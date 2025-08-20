// Build a flat list of buildings from both tables
ClearCollect(
    colBuildings;
    RenameColumns(
        ShowColumns('Data Seating1S'; "Building Name");
        "Building Name"; "Building"
    );
    RenameColumns(
        ShowColumns('Data Seating 2S'; "Building Name");
        "Building Name"; "Building"
    )
);


ClearCollect(
    colBuildings;
    RenameColumns(
        ShowColumns('Data Seating1S'; "Building Name");
        "Building Name"; "Building"
    );
    RenameColumns(
        ShowColumns('Data Seating 2S'; "Building Name");
        "Building Name"; "Building"
    )
);