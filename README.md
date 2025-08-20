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