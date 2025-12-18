# Changes to Spatial Media Metadata Injector GUI

## Summary
Modified the GUI to show the same directory view when saving that the current file has been picked from, improving user experience by reducing navigation steps.

## Files Changed
- `spatialmedia/gui.py`

## Detailed Changes

### File: `spatialmedia/gui.py`

#### Modified Method: `action_inject` (around line 184)

**Purpose:** When users select input files and then click "Inject metadata", the save dialog now opens in the same directory as the input files, rather than defaulting to the home directory or last used directory.

**Changes:**
- Added logic to extract the directory path from the first selected input file
- Pass this directory as the `initialdir` parameter to the `filedialog.askdirectory()` call
- Handle the case where no file is selected gracefully

**Code Changes:**
```python
# Before:
def action_inject(self):
    """Inject metadata into new save files."""
    # Ask for output directory instead of single file
    self.save_file = filedialog.askdirectory(title="Select Output Directory")
    if not self.save_file:
        return
    ...

# After:
def action_inject(self):
    """Inject metadata into new save files."""
    # Ask for output directory instead of single file
    # Use the directory of the first input file as the default
    initial_dir = os.path.dirname(self.in_file) if self.in_file else None
    options = {'initialdir': initial_dir} if initial_dir else {}
    self.save_file = filedialog.askdirectory(
        title="Select Output Directory",
        **options
    )
    if not self.save_file:
        return
    ...
```

## Benefits
1. **Improved User Experience:** Users no longer need to navigate back to the input file directory when saving processed files
2. **Consistency:** The save dialog now logically follows the input file location
3. **Efficiency:** Reduces clicks and navigation steps in the workflow

## Testing
- Verified that `os.path.dirname()` correctly extracts the directory path
- Confirmed that `filedialog.askdirectory()` accepts `initialdir` through the options dictionary
- Tested edge cases (None values) to ensure graceful handling

## Backward Compatibility
This change is fully backward compatible. The functionality remains the same; only the default directory shown in the save dialog has changed.
