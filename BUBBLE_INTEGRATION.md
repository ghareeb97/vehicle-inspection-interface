# Bubble.io Integration API

## Global Functions

### Initialize Inspection Module
```javascript
window.initializeInspectionModule(vehicleName, startingMileage, userId, inspectionId, mode, incomingDriverName, endMileage, submitCallback)
```

**Parameters:**
- `vehicleName` (string): Vehicle identifier
- `startingMileage` (number): Current odometer reading
- `userId` (string/number): Inspector ID
- `inspectionId` (string/number): Unique inspection ID
- `mode` (string): `'inspection'` or `'assignment-ending'` (default: `'inspection'`)
- `incomingDriverName` (string): Driver name for assignment-ending mode
- `endMileage` (number): Ending mileage for assignment-ending mode
- `submitCallback` (function): Handles form submission data

**Examples:**

*Inspection Mode:*
```javascript
window.initializeInspectionModule(
    "Toyota Camry 2023",
    45000,
    "user_123",
    "insp_456",
    "inspection",
    null,
    null,
    function(data) { console.log('Inspection data:', data); }
);
```

*Assignment Ending Mode:*
```javascript
window.initializeInspectionModule(
    "Toyota Camry 2023",
    45000,
    "user_123",
    "insp_456",
    "assignment-ending",
    "John Doe",
    45250,
    function(data) { console.log('Assignment ending data:', data); }
);
```

## Integration Modes

### Mode 1: Inspection Module (`'inspection'`)
Used when accessing from Bubble's inspection module where users start an inspection and select a vehicle.

**Setup:**
- User initiates inspection in Bubble app
- Vehicle data passed to HTML component
- Form pre-populated with vehicle information
- Mileage labels: "Mileage at Start" / "Mileage at End"
- Submission calls `bubble_fn_SaveInspectionResults()`

**Returns:** `output3` (fuel level)

### Mode 2: Assignment Ending (`'assignment-ending'`)
Used when ending a vehicle assignment/trip with inspection.

**Setup:**
- Vehicle assignment ending workflow
- Pre-populated with assignment data
- Includes incoming driver name field
- Mileage labels: "Starting Mileage" / "Ending Mileage"
- Submission calls `bubble_fn_save()`

**Returns:** `output1` (incoming driver name), `output2` (vehicle name), `output3` (fuel level)

### Mode 3: Standalone Mode
Used as independent inspection tool without pre-populated data.

**Setup:**
- Form loads with empty fields
- User enters all vehicle information manually
- Can be embedded in any Bubble page
- Submission data available for Bubble processing

## Submit Inspection Data
```javascript
window.submitInspectionToBubble()
```

**Returns:**
```javascript
{
    success: true,
    data: {
        outputlist1: ["Floor Mat", "Lighter", "Ash Tray", ...],     // Checklist item names
        outputlist2: ["Available", "Not checked", "Missing", ...], // Checklist item statuses
        outputlist3: ["Good condition", "", "Needs cleaning", ...], // Checklist item remarks
        output3: "Full",                                            // Fuel level
        // For assignment-ending mode only:
        output1: "John Doe",                                        // Incoming driver name
        output2: "Toyota Camry 2023",                              // Vehicle name
        userId: "user_123",
        vehicleName: "Toyota Camry 2023",
        startingMileage: 45000,
        fuelLevel: "Full",
        vehicleParts: { /* damage data */ },
        checklist: { /* inspection items */ },
        remarks: "Vehicle in good condition"
    }
}
```

## Data Structure
- **outputlist1**: Array of walk-around checklist item names (formatted)
- **outputlist2**: Array of walk-around checklist item statuses
- **outputlist3**: Array of walk-around checklist item remarks
- **output3**: Fuel level (string) - available in all modes
- **output1**: Incoming driver name (assignment-ending mode only)
- **output2**: Vehicle name (assignment-ending mode only)
- **Vehicle Info**: License plate, mileage, fuel level
- **Damage Data**: Part-specific damage types (D, S, R, K, C)
- **Checklist**: Complete checklist object with status and remarks per item
- **Remarks**: General inspector notes

## Other Available Functions
```javascript
window.switchVehicleInspection()    // Switch between vehicles
window.cancelInspection()           // Cancel current inspection
window.clearInspectionForm()        // Reset form state
```