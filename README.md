# fm-arrow-key-navigation

A FileMaker Pro technique demonstrating the use of arrow keys with layout script triggers to navigate between fields and records on a list based layout.

## How to Use

Open the file `ArrowKeyNav.fmp12` and start by clicking into any field.

Pressing → (right arrow) is similar to using the tab key and will move the cursor into the adjacent field on the right and higlight the contents of the field. The ← moves the cursor in the opposite (left) direction.

↑ ↓ keys will traverse the records.

## Why this Techinque Works Well

The arrow keys are grouped together on the keyboard and allow for quick field/record navigation.

## How is this Accomplished

Key presses are evaluated up by the `onLayoutKeystroke` layout script trigger, which is set to the `Nav Records` script. Details of the script are below.

Object names (fields) are hard coded and passed into the `Nav Records` script using an JSON Array, derived by the expression below, and stored in a variable named `$_objectNames`

```
JSONSetElement ( "[]" ;

[ 0 ; "author"   ; JSONString ];
[ 1 ; "bookTitle"   ; JSONString ];
[ 2 ; "pageLength" ; JSONString ];
[ 3 ; "publicationYear" ; JSONString ]

 )
```

For reference, the current field name is returned using the `Get(ActiveFieldName)` function and set to a variable named `$_fieldName`

The `$_objectNames` value, containing our script parameter JSON array, is then evaluated using the `$_fieldName` variable to return the field of origin or "index" and this value is stored into the `$_index` variable. In this demo, the number assigned to the `$_index` variable can only be in the range of [0-3]. When the → or ← arrow keys are pressed we know which direction to move the cursor into an adjacent field.

The key stroke is recorded using the `Code (Get(TriggerKeystroke))` in the `$_keyCode` variable.

A series of conditional block statements handles the logic to navigate the fields and records.
