Set(varBuildingSearch; Trim(txtBuildingSearch.Text));
Clear(colMerged);
Collect(colMerged; Filter('Data Seating1S'; IsBlank(varBuildingSearch) || StartsWith('Building Name'; varBuildingSearch)));
Collect(colMerged; Filter('Data Seating 2S'; IsBlank(varBuildingSearch) || StartsWith('Building Name'; varBuildingSearch)));
Navigate(scrResults; ScreenTransition.Fade)