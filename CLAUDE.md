# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Vehicle Inspection Interface** application built with vanilla HTML, CSS, and JavaScript. The application provides an interactive web-based interface for conducting vehicle inspections with the following key components:

- **Interactive Vehicle Diagram**: SVG-based vehicle diagram with clickable parts for detailed inspection
- **Dynamic Inspection Checklist**: Real-time checklist that updates based on selected vehicle components
- **Progress Tracking**: Visual progress indicators showing inspection completion status
- **Remarks System**: Text areas for adding inspection notes and observations

## File Structure

### Main Application Files
- `index.html` - Complete inspection interface with embedded CSS and JavaScript
- `vehicle.svg` - Interactive SVG vehicle diagram
- `Vehicle_inspection_template.pdf` - Reference template document
- `inspection_form.png` - Example screenshot/mockup

## Architecture

### Frontend Technology Stack
- **Vanilla JavaScript** - No frameworks, pure DOM manipulation
- **CSS Grid & Flexbox** - Responsive layout system
- **SVG Graphics** - Interactive vehicle diagrams with clickable regions
- **Single-file application** - All CSS and JavaScript embedded in index.html

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

## Bubble.io Integration

### Overview
The vehicle inspection interface supports integration with Bubble.io applications through global JavaScript functions. This allows the HTML component to be embedded in Bubble apps and receive/send data seamlessly.

### Integration Mode: Inspection Module
Used when accessing from Bubble's inspection module where users start an inspection and select a vehicle.

#### Global Functions

**Initialization:**
```javascript
window.initializeInspectionModule(vehicleName, startingMileage, userId, inspectionId, submitCallback)
```

**Parameters:**
- `vehicleName` (string): Complete vehicle name/identifier
- `startingMileage` (number): Current vehicle mileage/odometer reading
- `userId` (string/number): User ID of the person performing the inspection
- `inspectionId` (string/number): Unique inspection ID from Bubble database
- `submitCallback` (function): Callback function to handle form submission back to Bubble

**Data Submission:**
```javascript
window.submitInspectionToBubble()
```

Returns: `{ success: boolean, data: object, error?: string }`

### Usage in Bubble.io

#### HTML Element Setup
1. Add HTML element to Bubble page
2. Ensure element is visible (required for scripts)
3. Do NOT enable "Run as independent web page"
4. Enable "Expose HTML ID attributes" in Settings > General

#### Example Integration

**Method 1: Direct Function Call (Recommended)**
```javascript
// In Bubble workflow - Run JavaScript action
const vehicleName = "Toyota Camry 2023";
const startingMileage = 45000;
const userId = "user_12345";
const inspectionId = "insp_67890";

const submitCallback = function(inspectionData) {
    // Handle inspection data in Bubble
    console.log('Inspection completed:', inspectionData);
    // Trigger Bubble workflow or update database
};

window.initializeInspectionModule(vehicleName, startingMileage, userId, inspectionId, submitCallback);
```

**Method 2: Dynamic Data Insertion**
```html
<script>
// Insert dynamic data from Bubble database
window.initializeInspectionModule(
    [BUBBLE_VEHICLE_NAME],
    [BUBBLE_STARTING_MILEAGE],
    [BUBBLE_USER_ID],
    [BUBBLE_INSPECTION_ID],
    function(data) {
        // Handle form submission
        console.log('Form submitted:', data);
    }
);
</script>
```

### Data Validation
The integration automatically validates:
- Vehicle name as non-empty string
- Starting mileage as numeric value (0-9,999,999 range)  
- User ID as non-empty string or number
- Inspection ID as non-empty string or number
- Parameter type validation and sanitization

### Error Handling
- Invalid data throws descriptive error messages
- Failed submissions return error objects
- Console logging for debugging integration issues