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

### Initialize Inspection Checklist
```javascript
window.initializeInspectionChecklist(checklistItems)
```

**Parameters:**
- `checklistItems` (array): Array of checklist item objects

**Checklist Item Formats:**

*Option 1: Simple String Array (Recommended)*
```javascript
['Item 1', 'Item 2', 'Item 3']
```

*Option 2: Object Array (for custom IDs)*
```javascript
[
    { label: 'Item 1', id: 'custom-id-1' },
    { label: 'Item 2' }  // ID auto-generated
]
```

**Examples:**

*Ultra-Simple (Just Strings):*
```javascript
window.initializeInspectionChecklist([
    'Seat Belts',
    'Mirrors', 
    'Headlights',
    'Horn'
]);
```

*One-liner format:*
```javascript
window.initializeInspectionChecklist(['Floor Mat', 'Seat Belts', 'Mirrors', 'Headlights']);
```

*Mixed Languages (Strings):*
```javascript
window.initializeInspectionChecklist([
    'أحزمة الأمان',    // Arabic
    'Cinturones',      // Spanish  
    'Miroirs',         // French
    'हॉर्न'             // Hindi
]);
```

*Object Format (when you need specific IDs):*
```javascript
window.initializeInspectionChecklist([
    { id: 'seat-belts', label: 'Seat Belts' },
    { id: 'mirrors', label: 'Mirrors' },
    'Headlights',  // Mixed: string gets auto ID
    'Horn'
]);
```

*Custom Checklist from Bubble (Arabic):*
```javascript
window.initializeInspectionChecklist([
    { id: 'seat-belts', label: 'أحزمة الأمان' },
    { id: 'mirrors', label: 'المرايا' },
    { id: 'headlights', label: 'الأضواء الأمامية' },
    { id: 'horn', label: 'البوق' }
]);
```

*Variable Item Counts (1 item):*
```javascript
window.initializeInspectionChecklist([
    { label: 'Emergency Kit Only' }
]);
```

*Variable Item Counts (large checklist):*
```javascript
window.initializeInspectionChecklist([
    { label: 'Seat Belts' }, { label: 'Mirrors' }, { label: 'Horn' },
    { label: 'Headlights' }, { label: 'Tail Lights' }, { label: 'Turn Signals' },
    { label: 'Spare Tire' }, { label: 'Tool Kit' }, { label: 'Jack' },
    { label: 'Fire Extinguisher' }, { label: 'First Aid Kit' }, { label: 'Reflectors' },
    { label: 'Floor Mats' }, { label: 'Air Freshener' }, { label: 'Documents' },
    { label: 'GPS Device' }, { label: 'Phone Charger' }, { label: 'Water' },
    { label: 'Cleaning Supplies' }, { label: 'Emergency Flare' }
    // ... any number of items
]);
```

*Empty checklist (no items):*
```javascript
window.initializeInspectionChecklist([]);
```

**Returns:** `{ success: boolean, error?: string }`

**Usage Notes:**
- **Variable item count**: Pass any number of items (1, 5, 20, 50+) - the system adapts automatically
- **Dynamic rendering**: Grid layout adjusts to accommodate any quantity
- **No limits**: Can handle both small (1-3 items) and large (100+ items) checklists
- Call this function to set custom checklist items from your Bubble database
- If not called, the form will use default checklist items (15 items)
- Must be called after the DOM is loaded
- Each item will render with Available/Missing/N/A checkboxes and a remarks field
- **Empty checklist supported**: Pass `[]` for no checklist items

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