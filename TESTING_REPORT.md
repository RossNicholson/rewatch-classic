# Rewatch Classic - Testing & Bug Analysis Report

## Static Code Analysis Summary

This document outlines potential bugs and issues found through static code analysis of the Rewatch Classic addon.

## Critical Issues Found

### 1. InCombatLockdown() Type Inconsistency
**Severity**: High  
**Location**: Multiple locations in `Rewatch.lua` and `Rewatch_options.lua`

**Issue**: In MoP Classic, `InCombatLockdown()` returns a boolean (true/false), not a number. However, several places in the code check `InCombatLockdown() == 1` which will always be false.

**Affected Lines**:
- `Rewatch.lua:597` - `if(InCombatLockdown() == 1) then return -1; end;`
- `Rewatch.lua:982` - `if((InCombatLockdown() == 1) or ...)`
- `Rewatch.lua:1188` - `if(InCombatLockdown() == 1) then rewatch_Message(...)`
- `Rewatch.lua:1344, 1360, 1369, 1375, 1392` - Multiple `== 1` checks
- `Rewatch.lua:1628` - `if(InCombatLockdown() == 1) then rewatch_Message(...)`
- `Rewatch.lua:1880` - `if(InCombatLockdown() ~= 1) then`
- `Rewatch_options.lua:317` - `if(InCombatLockdown() == 1) then`
- `Rewatch_options.lua:543` - `if((not get) and (InCombatLockdown() ~= 1)) then`

**Fix Required**: Replace all `InCombatLockdown() == 1` with `InCombatLockdown()` and `InCombatLockdown() ~= 1` with `not InCombatLockdown()`.

**Impact**: Combat lockdown protection may not work correctly, potentially causing errors during combat when trying to modify frames.

---

### 2. UNIT_THREAT_SITUATION_UPDATE Event Handler
**Severity**: High  
**Location**: `Rewatch.lua:1635-1656`

**Issue**: The `UNIT_THREAT_SITUATION_UPDATE` event passes a unit token (string like "player", "target", "party1", etc.), not a GUID. The code incorrectly uses `UnitName(unitGUID)` where `unitGUID` is actually a unit token.

**Current Code**:
```lua
elseif(event == "UNIT_THREAT_SITUATION_UPDATE") then
    local unitGUID = ...;  -- This is actually a unit token, not a GUID
    if(unitGUID) then
        playerId = rewatch_GetPlayer(UnitName(unitGUID));  -- WRONG: unitGUID is a unit token
```

**Fix Required**: Use the unit token directly with `UnitGUID()` to get the GUID, or iterate through players to find matching unit token.

**Impact**: Threat updates may not work correctly, and player name lookups will fail.

---

### 3. PLAYER_ROLES_ASSIGNED Event Handler
**Severity**: Medium  
**Location**: `Rewatch.lua:1658-1670`

**Issue**: The `PLAYER_ROLES_ASSIGNED` event fires without parameters when roles are assigned. The code incorrectly tries to extract a parameter and use `UnitName(unitGUID)`.

**Current Code**:
```lua
elseif(event == "PLAYER_ROLES_ASSIGNED") then
    local unitGUID = ...;  -- This event doesn't pass parameters
    if(unitGUID) then
        playerId = rewatch_GetPlayer(UnitName(unitGUID));  -- WRONG
```

**Fix Required**: Iterate through all group members and update their role icons, or use a different approach to detect role changes.

**Impact**: Role icons may not update when roles are assigned in a group/raid.

---

## Medium Priority Issues

### 4. Potential Nil Access in roleIcon
**Severity**: Medium  
**Location**: `Rewatch.lua:1665-1668`

**Issue**: The code uses `roleIcon` without checking if it exists for the specific player frame. If `roleIcon` was not created for a particular frame, this will cause an error.

**Fix Required**: Check if `val["Frame"]["roleIcon"]` or similar exists before using it, or ensure all frames have a roleIcon created.

---

### 5. Missing Nil Check in FindUnitBuff
**Severity**: Low  
**Location**: `Rewatch.lua:32-47`

**Issue**: The `findUnitBuff` function doesn't validate that the `unit` parameter is a valid unit token before calling `UnitBuff`.

**Fix Required**: Add validation: `if not unit or not UnitExists(unit) then return nil; end`

---

### 6. Potential Memory Leak in Options Frame
**Severity**: Low  
**Location**: `Rewatch_options.lua:343-353`

**Issue**: A hook frame is created for `ADDON_LOADED` event but may not always unregister. While it does unregister in the success case, if `Blizzard_InterfaceOptions` never loads, the event remains registered.

**Fix Required**: Consider adding cleanup on addon unload or using a different approach.

---

## Static Analysis Tests Performed

### ✅ Syntax Checking
- **Result**: All Lua syntax is valid
- **Method**: Manual review and pattern matching

### ✅ API Compatibility
- **Result**: Most APIs are compatible, except issues noted above
- **Method**: Cross-referenced with MoP Classic API documentation

### ✅ Frame Creation
- **Result**: All frames use `BackdropTemplate` correctly
- **Method**: Verified all `CreateFrame` calls for frames that need backdrop

### ✅ Event Registration
- **Result**: Events are properly registered
- **Method**: Verified event registration in initialization code

### ⚠️ Combat Lockdown Checks
- **Result**: Multiple incorrect checks found (see Issue #1)
- **Method**: Searched for all `InCombatLockdown` usages

### ⚠️ Nil Safety
- **Result**: Most places check for nil, but some improvements possible
- **Method**: Searched for direct property access without nil checks

---

## Recommended Testing Checklist

### In-Game Testing

1. **Combat Lockdown Testing**
   - [ ] Join combat and try to move frames
   - [ ] Join combat and try to add/remove players
   - [ ] Join combat and try to use `/rewatch` commands
   - [ ] Verify no "ADDON_ACTION_BLOCKED" errors occur

2. **Threat System Testing**
   - [ ] Form a party/raid
   - [ ] Pull mobs and verify threat colors appear on player frames
   - [ ] Verify threat colors update correctly
   - [ ] Check for any errors in chat

3. **Role Assignment Testing**
   - [ ] Form a raid group
   - [ ] Assign roles (tank, healer, DPS)
   - [ ] Verify role icons appear on player frames
   - [ ] Verify role icons update when roles change

4. **Lifebloom Stack Testing**
   - [ ] Cast Lifebloom on a target multiple times
   - [ ] Verify bar color changes for 1, 2, and 3 stacks
   - [ ] Verify stack count is accurate

5. **Regrowth Lifebloom Refresh Testing**
   - [ ] Cast Lifebloom on a target
   - [ ] Cast Regrowth on the same target
   - [ ] Verify Lifebloom timer refreshes correctly

6. **Frame Management Testing**
   - [ ] Add players manually with `/rewatch add`
   - [ ] Remove players from group and verify auto-removal
   - [ ] Test `/rewatch default` command
   - [ ] Test `/rewatch sort` command

7. **Error Handling Testing**
   - [ ] Test with invalid commands
   - [ ] Test with non-existent player names
   - [ ] Test with special characters in names
   - [ ] Monitor error output in chat

---

## Suggested Fixes Priority

1. **P0 (Critical)**: Fix InCombatLockdown() checks - affects combat safety
2. **P0 (Critical)**: Fix UNIT_THREAT_SITUATION_UPDATE handler - affects threat display
3. **P1 (High)**: Fix PLAYER_ROLES_ASSIGNED handler - affects role icon display
4. **P2 (Medium)**: Add nil checks for roleIcon
5. **P3 (Low)**: Improve nil validation in findUnitBuff
6. **P3 (Low)**: Review event cleanup

---

## Automated Testing Limitations

Since this is a World of Warcraft addon, automated testing is limited. The following cannot be tested without in-game execution:

- Actual WoW API behavior
- Frame rendering and positioning
- Combat event handling
- Network synchronization (party/raid events)
- Performance under load
- Memory usage over time

All critical issues should be tested in-game after fixes are applied.
