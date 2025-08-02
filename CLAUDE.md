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
- `index.html` - Primary inspection interface with embedded styles and JavaScript (~44k lines)
- `template_diagram.html` - Template/component version with React integration hooks (~41k lines)

### Assets
- `vehicle.svg` - Vehicle diagram SVG file
- `Vehicle_inspection_template.pdf` - Reference template document
- `inspection_example.png` & `inspection_form.png` - Example screenshots/mockups

## Architecture

### Frontend Technology Stack
- **Vanilla JavaScript** - No frameworks, pure DOM manipulation
- **CSS Grid & Flexbox** - Responsive layout system
- **SVG Graphics** - Interactive vehicle diagrams with clickable regions
- **React Integration** (template_diagram.html only) - Uses React hooks via UB framework

### Key JavaScript Functionality
- **Part Selection System**: Click handlers for vehicle diagram components
- **Progress Tracking**: Calculates completion percentage based on inspected parts
- **Form State Management**: Tracks inspection status for each vehicle component
- **Tooltip System**: Hover tooltips for vehicle parts identification
- **Data Export**: Generates inspection reports in various formats

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

1. **Opening files directly in browser** - Open `index.html` or `template_diagram.html`
2. **Live reload** - Use browser developer tools or live server extension
3. **Testing** - Manual testing in browser, no automated test framework

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
    vehicleInfo: { /* vehicle details */ },
    inspectionItems: { /* part-specific inspection results */ },
    remarks: "general notes",
    progress: { completed: X, total: Y, percentage: Z }
}
```

### React Integration (template_diagram.html)
- Uses UB framework for parent-child communication via postMessage
- Provides hooks: useData(), triggerEvent(), updateValue()
- Connects to external React applications when used as embedded component