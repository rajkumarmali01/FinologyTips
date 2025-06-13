import streamlit as st
import pandas as pd
from io import BytesIO

def main():
    st.title("Employee Seating Location Mapper 🏢")

    # File Uploaders
    main_file = st.file_uploader("Main Sheet (Excel)", type=["xlsx"])
    atwork_file = st.file_uploader("Atwork - All Floor Mapping (Excel)", type=["xlsx"])
    gcc_ach_file = st.file_uploader("GCC_ACH Seating (Excel)", type=["xlsx"])
    seating_ch6_file = st.file_uploader("Seating Location_CH6 (Excel)", type=["xlsx"])

    if main_file and atwork_file and gcc_ach_file and seating_ch6_file:
        try:
            # Load main sheet
            main_df = pd.read_excel(main_file)

            # Load and process Atwork sheet
            atwork_df = pd.read_excel(atwork_file)
            atwork_df = atwork_df[['EMPLOYEE ID', 'Workstation', 'Floor', 'Type']]
            atwork_df = atwork_df.rename(columns={'EMPLOYEE ID': 'Employee Code'})

            # Load and process GCC_ACH Seating
            gcc_ach_df = pd.read_excel(gcc_ach_file)
            gcc_ach_df = gcc_ach_df[['Employee Code', 'Sticker Display Code', 'Description']]

            # Load and process Seating Location_CH6
            seating_ch6_df = pd.read_excel(seating_ch6_file)
            seating_ch6_df = seating_ch6_df[['Employee Code', 'Floor', 'Workstation No', 'Workstation type']]

            # Initialize new columns in main_df
            main_df['Seating Location'] = None
            main_df['Seating Location Source'] = None
            main_df['Floor'] = None
            main_df['Workstation Number'] = None
            main_df['Workstation Type'] = None

            # Map data from each source
            for emp_code in main_df['Employee Code']:
                # Check Seating Location_CH6 first
                ch6_row = seating_ch6_df[seating_ch6_df['Employee Code'] == emp_code]
                if not ch6_row.empty:
                    main_df.loc[main_df['Employee Code'] == emp_code, 'Seating Location'] = f"Seating Location: CH6"
                    main_df.loc[main_df['Employee Code'] == emp_code, 'Seating Location Source'] = 'CH6'
                    main_df.loc[main_df['Employee Code'] == emp_code, 'Floor'] = ch6_row['Floor'].values[0]
                    main_df.loc[main_df['Employee Code'] == emp_code, 'Workstation Number'] = ch6_row['Workstation No'].values[0]
                    main_df.loc[main_df['Employee Code'] == emp_code, 'Workstation Type'] = ch6_row['Workstation type'].values[0]
                else:
                    # Check Atwork sheet if CH6 not found
                    atwork_row = atwork_df[atwork_df['Employee Code'] == emp_code]
                    if not atwork_row.empty:
                        main_df.loc[main_df['Employee Code'] == emp_code, 'Seating Location'] = f"Seating Location: Atwork"
                        main_df.loc[main_df['Employee Code'] == emp_code, 'Seating Location Source'] = 'Atwork'
                        main_df.loc[main_df['Employee Code'] == emp_code, 'Floor'] = atwork_row['Floor'].values[0]
                        main_df.loc[main_df['Employee Code'] == emp_code, 'Workstation Number'] = atwork_row['Workstation'].values[0]
                        main_df.loc[main_df['Employee Code'] == emp_code, 'Workstation Type'] = atwork_row['Type'].values[0]

            # Final column selection
            final_columns = [
                'Employee Code',
                'Name of Employee',
                'Employee Type',
                'Location Name',
                'Date of Joining',
                'Department 1.0',
                'Location Code',
                'Location City',
                'Seating Location',
                'Floor',
                'Workstation Number',
                'Workstation Type'
            ]
            
            # Filter and reorder columns
            main_df = main_df[[col for col in final_columns if col in main_df.columns]]

            # Display results
            st.success("✅ Data merged successfully!")
            st.dataframe(main_df)

            # Download functionality
            output = BytesIO()
            with pd.ExcelWriter(output, engine='xlsxwriter') as writer:
                main_df.to_excel(writer, sheet_name='Main', index=False)
            output.seek(0)
            
            st.download_button(
                label="📥 Download Merged Data",
                data=output,
                file_name="employee_seating_details.xlsx",
                mime="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
            )

        except Exception as e:
            st.error(f"❌ Error: {str(e)}")

if __name__ == "__main__":
    main()
