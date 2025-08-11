# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Vehicle Walk-Around Inspection Form** - a digital inspection tool featuring an interactive vehicle diagram for documenting damage, completing safety checklists, and recording vehicle condition data. Built with vanilla HTML, CSS, and JavaScript for maximum compatibility with Bubble.io integration.

### Core Functionality
- **Interactive SVG Vehicle Diagram**: Clickable parts for damage documentation with visual feedback
- **Fuel Level Indicator**: Visual gauge with preset buttons and custom input
- **Walk-Around Checklist**: Safety equipment and vehicle component verification
- **Damage Classification**: Five damage types (D-Dent, S-Scratches, R-Rust, K-Broken, C-Chips)
- **Multi-Mode Operation**: Supports inspection and assignment-ending workflows
- **Progress Tracking**: Real-time completion percentage calculation

## File Structure

### Main Application Files
- `index.html` - Complete inspection interface with external CSS reference
- `gem.css` - Unified stylesheet containing all application styling (consolidates previous styles.css)
- `preview.html` - Preview interface for inspection results (uses legacy styles.css reference)
- `vehicle.svg` - Interactive SVG vehicle diagram
- `Vehicle_inspection_template.pdf` - Reference template document
- `do not use.css` - Deprecated CSS file (kept for reference)

## Architecture

### Frontend Technology Stack
- **Vanilla JavaScript** - No frameworks, pure DOM manipulation
- **CSS Grid & Flexbox** - Responsive layout system
- **SVG Graphics** - Interactive vehicle diagrams with clickable regions
- **Unified CSS** - Single stylesheet (gem.css) architecture for all Bubble.io HTML elements
- **Preview System** - Separate preview interface for inspection results

### Key JavaScript Functionality
- **Part Selection System**: Click handlers for vehicle diagram components with damage tracking
- **Progress Tracking**: Real-time calculation of completion percentage based on form fields
- **Form State Management**: Tracks inspection status for each checklist item
- **Tooltip System**: Hover tooltips for vehicle parts identification
- **Damage Documentation**: Modal system for recording vehicle damage with descriptions
- **Data Collection**: Comprehensive form data aggregation for inspection reports

### CSS Architecture
- **Component-based styling** with clear class naming conventions
- **Responsive design** using CSS Grid for main layout
- **Interactive states** for clickable diagram elements (.maplink, .active)
- **Custom properties** for consistent theming

### SVG Diagram System
- Vehicle parts have unique IDs (e.g., "FRONT_BUMPER", "FRONT_HEADLAMP")
- CSS classes for styling (.st0, .st1, .maplink)
- Event handlers for click/hover interactions
- Visual feedback for selected/inspected parts

## Development Commands

This project uses no build system or package manager. Development is done by:

1. **Opening files directly in browser** - Open `index.html` in web browser
2. **Live reload** - Use browser developer tools or live server extension (e.g., VS Code Live Server)
3. **Testing** - Manual testing in browser, no automated test framework
4. **File serving** - Any local HTTP server (Python: `python -m http.server`, Node: `npx serve`)

## Key Implementation Details

### Vehicle Part Interaction
- Parts are defined as SVG paths with specific IDs
- JavaScript maps part IDs to inspection checklist items
- Active selection updates both visual state and form data

### Progress Calculation
- Tracks completed inspections vs total required inspections
- Updates progress bar and percentage display in real-time
- Validates completion before allowing form submission

### Data Structure
The inspection data follows this pattern:
```javascript
inspectionData = {
    vehicleInfo: { /* customer and vehicle details */ },
    checklist: { /* inspection item statuses */ },
    damageData: { /* vehicle damage documentation */ },
    remarks: "general inspection notes",
    progress: { totalItems: X, completedItems: Y, completionPercentage: Z }
}
```

### Main JavaScript Functions
- `loadVehicleSVG()` - Loads and sets up interactive vehicle diagram
- `partClick()` - Handles vehicle part selection and damage modal
- `updateProgress()` - Calculates and updates inspection completion percentage
- `collectFormData()` - Aggregates all form data for submission
- `setupFormEventListeners()` - Initializes all form interaction handlers
- `showTooltip()` / `hideTooltip()` - Manages hover tooltips for vehicle parts
- `testInspectionMode()` - Development function to test Bubble.io integration

## Bubble.io Integration Architecture

### Integration Modes
The application supports three distinct operational modes:

1. **Inspection Mode** (`'inspection'`): Standard vehicle inspection workflow
   - Driver section hidden
   - Mileage labels: "Mileage at Start" / "Mileage at End"
   - Calls `bubble_fn_SaveInspectionResults()` on submission

2. **Assignment Ending Mode** (`'assignment-ending'`): End-of-trip inspection with driver handover
   - Driver section visible with incoming driver field
   - Mileage labels: "Starting Mileage" / "Ending Mileage"
   - Calls `bubble_fn_save()` on submission

3. **Standalone Mode**: Independent operation without Bubble integration

### Primary Integration Function
```javascript
window.initializeInspectionModule(vehicleName, startingMileage, userId, inspectionId, mode, incomingDriverName, endMileage, submitCallback)
```

**Critical Implementation Details:**
- Mode parameter determines UI layout and submission behavior
- Driver section visibility automatically managed based on mode
- Form data structure adapts to mode requirements
- Validation handles different parameter combinations per mode

### HTML Element Requirements in Bubble
- Element must be **visible** (scripts won't execute if hidden)
- **DO NOT** enable "Run as independent web page"
- **Enable** "Expose HTML ID attributes" in Settings > General
- Use HTML element, not iframe or embed

### Data Flow Architecture
1. **Initialization**: Bubble passes parameters to global function
2. **Form Population**: UI updates based on mode and provided data
3. **User Interaction**: Inspector completes form sections
4. **Data Collection**: `collectFormData()` aggregates all form inputs
5. **Submission**: Mode-specific Bubble function called with formatted data

## Recent Changes and Architecture Notes

### CSS Architecture Evolution
- **Current**: `gem.css` - Unified stylesheet containing all application styles
- **Legacy**: `preview.html` still references `styles.css` (needs updating for consistency)
- **Deprecated**: `do not use.css` - Contains older styles kept for reference

### Critical Implementation Notes
- **No Build System**: Direct HTML/CSS/JS files, no package manager or build process
- **Embedded SVG**: Vehicle diagram is embedded directly in HTML, not loaded externally
- **Inline Styles with Responsive CSS**: Uses inline styles with CSS media queries for responsive behavior
- **Event Delegation**: Fuel buttons and SVG parts use direct event listeners (not delegation)
- **Global State**: `bubbleConfig` and `inspectionData` objects manage application state

### Debugging and Common Issues
- **JavaScript Syntax Errors**: Check browser console; syntax errors break entire page functionality
- **Missing Elements**: SVG and fuel buttons not appearing indicates DOM loading or JavaScript errors
- **Fuel Buttons Not Clickable**: Ensure `pointer-events: auto` and proper z-index on buttons
- **Driver Section Visibility**: Automatically controlled by mode parameter in initialization
- **Form Data Collection**: All form inputs must have proper IDs for `collectFormData()` to work

### Global JavaScript API Functions
```javascript
window.initializeInspectionModule()  // Primary integration entry point
window.submitInspectionToBubble()    // Form submission with mode-specific callbacks
window.switchVehicleInspection()     // Switch between vehicles in same session
window.cancelInspection()            // Cancel current inspection
window.clearInspectionForm()         // Reset all form data and UI state
window.handleDamageButtonClick()     // Handle damage type selection (D,S,R,K,C)
window.testChecklistCollection()     // Development function for testing
window.testDamageButtons()           // Development function for damage testing
```

### Fuel Level System Architecture
- **Visual Gauge**: CSS-based height animation with primary color (#006ab4)
- **Preset Buttons**: 9 preset levels from Empty to Full with fractional options
- **Custom Input**: Text field for non-standard fuel levels
- **Responsive Layout**: 3→2 column grid on mobile with larger touch targets
- **State Management**: Updates visual gauge, display text, and hidden form input