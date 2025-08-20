Set(varBuildingSearch; Trim(txtBuildingSearch.Text));

// Collect rows from both tables (filtering based on text input)
Clear(colMerged);

Collect(
    colMerged;
    Filter('Data Seating1S'; StartsWith('Building Name'; varBuildingSearch))
);
Collect(
    colMerged;
    Filter('Data Seating 2S'; StartsWith('Building Name'; varBuildingSearch))
);

// Navigate to results screen
Navigate(scrResults; ScreenTransition.Fade);