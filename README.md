# **Virgo Potenz Hospital** Patient-Waiting-List-Report

## Problem Statement

Virgo Potenz Hospital,  a healthcare facility where patients receive medical treatment, surgical interventions, and nursing care.
Its primary purpose is to restore health, alleviate suffering, and promote well-being. The company’s executive board wishes to establish insights in thier patient waiting list to enable them make the following decisions
1. **Efficient Resource Allocation**:
* Insight into Demand: Waiting list analysis helps hospitals understand the demand for various services. By identifying which procedures have longer wait times, hospitals can allocate resources more efficiently.
* Optimized Scheduling: Hospitals can prioritize patients based on urgency, ensuring that critical cases receive prompt attention while managing non-urgent cases effectively.
2. **Improved Patient Experience**:
* Reduced Wait Times: By analyzing waiting lists, hospitals can identify bottlenecks and implement strategies to reduce wait times. This enhances patient satisfaction and overall experience.
* Transparency: Patients appreciate knowing their expected wait times. Transparent communication about delays fosters trust and reduces anxiety.
3. **Enhanced Quality of Care**:
* Timely Interventions: Reducing waiting times ensures timely access to medical care. Early interventions often lead to better outcomes.
* Risk Management: Analyzing waiting lists helps hospitals identify patients at risk due to prolonged waits. Prioritizing these cases minimizes adverse events.
4. **Operational Efficiency**:
* Resource Planning: Hospitals can allocate staff, operating rooms, and equipment more effectively by understanding demand patterns.
* Staff Productivity: Efficient scheduling reduces idle time for healthcare professionals, improving overall productivity.
5. **Financial Impact**:
 * Revenue Optimization: Effective management of waiting lists ensures that revenue-generating procedures are scheduled optimally.
* Cost Control: By minimizing unnecessary delays, hospitals can control costs associated with prolonged stays and complications.


**The overall objectives for this Project are as follows** 

**Project Goals**

1. Track current patient status of patient waiting List
2. Analyze historical monthly trend of waiting list inpatients and outpatient category
3. Detailed specialty level and age Profile Analysis

**Data Scope**

2018-2021


**Metrics Required**

1. Average and Median Waiting List
2. Current Total Waiting List

**Views Required**
1. Summary page
2. Detailed Page granular activities





## Stage 1: Extract Transform and Load 

### Steps followed 

**Step 1: Launch the Power BI app and Load data**

1. Launch the Power BI Desktop in your local Machine

2. The File containing the dataset for this project are contained in 2 Folders labelled 
* Inpatient and 
* Outpatient 
with each folder containing 4 excel files representing each year.

3. check all the 4 excel files and accertain they have the same headers. This is to enable joining(combine) the 4 files effectively

4. Load the Inpatient Folder into Power BI using the Folder data connector. 
![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/f057c4a9-2f3e-4512-868a-7738dc89121e)

and select *combine*, in the dropdown, select *combine & Load*. 

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/190360ed-1e54-4221-b70d-3592a2686e83)

5. Repeat same for Outpatient folder


The two Datasets have successfully been loaded into the Power Bi.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/8442d2d6-eeac-4d4a-87f5-1c3776e5518b)


6. on checking thier datatypes notiice the Archive_Date is not in the Date datatype. Perform the following to correect the anomaly

        1. Go to the Table View and highlight the Archive_Date column
        2. Go to the Datatype field and select Date

7. Inspect all the columns to ensure the datatypes fits the columns


8. Select the Transform Data option to enter the Power Query Editor.
Observe that we have imorted the following Tables into the Power Query

        * Inpatient
        * Outpatient


![Presentation1](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/dcba22f7-d780-47f8-b273-867cfaefa88d)     

**Step 2: Perform data Transformation on the Queries**

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/ba1aa321-baf4-424a-bb10-b231b785fe62)

1. Optimize the dataset by assigning datatypes accordingly. Ensure that every column in all the Queries are in their correct datatype.


2. Join the two Datasets into 1 using the Append method by following these Steps
       
        1. Check if the 2 Tables Have the same Headings. Notice that Inpatient table has a 
        column = specialty_Name 
        while Outpatient table has 
        column = Specialty
        change the name for Outpatient to specialty_Name.

        2. Theres a column in the Inpatient dataset(case_type) that is not in Outpatient dataset. 
        A closer look on the case_type column shows it only contains "Inpatient" & "Day case" 
        which shows the status of the inpatients while it was not created in Outpatient query
        because all outpatients have same status which is "Outpatient".
        Because we are joining the 2 Queries, we need create the case_type column in the Outpatient query 
        and populate it with "Outpatient". The following shows how to create the new column in the Outpatient query.

        1. Navigate to Add column tab and select Custom Column
        2. New column name: case_type
           Custom column formula: "Outpatient"
           press ok to create the new column.

        3. Drag the column to the exact same position like in the Inpatient query after the specialty_Name.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/cce324e3-13ec-48b4-b0a7-1ef7c810361d)

        4. On the Home Tab, Go to Append Queries dropdown and select Append Queries as New.
        * First Table: Inpatient
        * Second Table: Outpatient
        * Rename the new Query from Append_1 to All_Data

3. Check the columns by clicking on the headers to inspect how the values are represented whether there re some repetitions. In the `Time_Bands` column, there are irregularities in there values as numbers like **18 Months +**, **18 + Months** which represents the same thing and  **0-3 months**, **3-6 Months**, **6-9months** etc appearing twice instead of once. 
![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/7427f8a7-8509-4196-b967-3c31c8c153f4)

we perform the following Transformation on the column

        1. Replace 18+ Months with 18 Months +
                * Go to Transform Tab and select Replace values
                * Values to find: 18+ Months
                  Replace with: 18 Months + 
        2. Use Trim to tidy every other anomalies in the column like double spav=cing etc 
                * On the Transform Tab, select Format from the Text Column group
                * select Trim from the Format dropdown

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/1464896b-aa75-4297-ac54-7f9e93905605)


4. Close and Apply to return to Power BI Desktop.


#### **Step 3: Data Modelling**
1. Import another csv Dataset `Mapping_Specialty` into Power BI Desktop Using csv Data connector and select Transform to open in Power Query.

2. Notice that the 1st row is actually the Headers. change the first row to Headers

        * select the first box dropdown in the Table that has a Table icon and select Use First Rows as Headers

3. Close and Apply

4. Go to the Model view of the Power BI Desktop


5. There are 4 Tables in the Model view, Hide Inpatient and Outpatient from the Reprt view to avoid confusion 

        select the ellipsis and select hide in report view

6. Establish a relationship between the `All_Data` and the `Mapping_Specialty` Tables. Notice Power BI already established a relationship between specialty_Name and Specialty respectively.

7. Delete any other relationship other than the established one between `All_Data` & `Mapping_Specialty`

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/d3533486-767d-4377-a3ac-e3b925c0b1fd)

## Stage 2 Analysis:
 **Step 1: Create Tables and Measures**

 1. Create a table `Calculation Method` that will contain *Average* and *Median* mode which are the bases for the Calculations

        Navigate to home tab and select Enter data,
        a new Table appears, we rename it Calculation Method
        Rename the column: Calc Method
        1st row: Average
        2nd row: Median
        Load

        

 2. Create a table where all the measures will be stored.

         Navigate to home tab and select Enter data,
         a new Table appears, we rename it Key_Measures
         Load




        Note*: The + 0 is to make the expression not to return blank when subjected to certain situations using Slicers.


3. Create the Median Wait List Key_Measure
        
        Median Wait List = MEDIAN(All_data[Total])

4. Create the Average Wait List Key_Measure

        Average Wait List = AVERAGE(All_data[Total])


5. Create a Measure that will switch between *Average Wait List* and *Median Wait List* depending on the selected Row in the Calc Method of the `Calculation Method` Table.

        Avg/Med Wait List = SWITCH(VALUES('Calculation Method'[Calc Method]), "Average", [Average Wait List], "Median", [Median Wait List])

6. Create the Latest Month Wait List Measure

        Latest month Wait List = CALCULATE(SUM(All_data[Total]), All_data[Archive_Date] = MAX(All_data[Archive_Date])) + 0

7. Create the Previous Year Latest Month Wait List Measure

        PY Latest Month Wait List = CALCULATE(SUM(All_data[Total]),
        All_data[Archive_Date] = EDATE(MAX(All_data[Archive_Date]),-12)) + 0

        
8. Create a Background display Measure that will display a message(No Data to Display for the selected Specialization) instead of displaying **blank**

        Background disp = IF(ISBLANK(CALCULATE(SUM(All_data[Total]),All_data[Case_Type] <> "Outpatient")), 
        "No Data to Display for the selected Specialization", "")

9. Create another Background display Measure that will display a message(No Data to Display for the selected Specialization) instead of displaying **blank**

        Background disp2 = IF(ISBLANK(CALCULATE(SUM(All_data[Total]),All_data[Case_Type] = "Outpatient")), "No Data for the selected category", "")

10. create the Total Sales Measure

        right click on Key Measure and select New measure
        write the following DAX expression

                Latest month Wait List = CALCULATE(SUM(All_Data[Total]),
                 All_Data[Archive_Date] = MAX(All_Data[Archive_Date])) + 0


11. Create a dynamic Title that will change inline with the selected Row  in the Calc Method of the `Calculation Method` Table.

        Dynamic Title = SWITCH(VALUES('Calculation Method'[Calc Method]), "Average", "Key Indicators- Patient Wait List: Average", 
        "Median", "Key Indicators- Patient Wait List: Median")

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/79fafe8e-31d0-468c-b83e-368d33b39fc9)


 **Step 2: Format the Canva**
 

1. Choose a Canvas Settings Type size of 16.9

2. Choose a Canvas Background color of Black and set Transparency to 0%

**Step 3: Create visualizations Page1(Summary)**

1. Create a Latest Month Card visual using the Card visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the Latest month Wait List Measure to the fields well 


2. Create a Previous Year Latest Month Card visual using the Card visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the PY Latest month Wait List Measure to the fields well
![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/4dce94b0-a8ad-4739-8533-acadc56feb52)

3. Create a Archive_Date Slicer using the Slicer visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the Archive_Date to the fields well
![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/92ffee14-205e-4184-8c31-13dd64ddb42a)


4. Create a Case_Type Slicer using the Slicer visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the Case_Type to the fields well
        3. Slicer Setting Style: dropdown

5. Create a specialty_Name Slicer using the Slicer visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the specialty_Name to the fields well
        3. Slicer Setting Style: dropdown

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/8757c5c4-2bc8-415b-8915-5f3d839b3b26)

6. Create a Calculation Method Slicer using the Slicer visualization. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the `Calculation Method`[Calc Method] to the fields well
        3. Slicer Setting Style: Tile
![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/929ca6c6-81b1-4d9c-962c-3345a2b05e21)


7. Create a Avg/Med Wait List by Case_Type visual using the Donot Chart visualization. This is achieved by the following Steps.

        1. select the Donot Chart from the visualization pane
        2. drag the All_Data[Case_Type] to the fields well legend 
        3. drag the ` Avg/Med Wait List` Measure to the fields well 

 ![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/c52c06a5-b203-4ba2-b71f-aa64f4ca6622)

8. Create a Avg/Med Wait List by Time_Bands and Age_Profile visual using the Stacked Column Chart visualization. This is achieved by the following Steps.

        1. select the Stacked Column Chart from the visualization pane
        2. drag the `Avg/Med Wait List` Measure to the fields well y-axis 
        3. drag the `All_Data[Time_Bands] to the fields well x-axis
        4. drag the `All_Data[Age_Profile] to the fields well legend

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/fb551056-da17-4fb0-a69f-adf1e77a30bf)

9. Create a Multiple card visual to highlight the 5 Top specialty_Name with respect to `Avg/Med Wait List` .

        1. select the Multiple card from the visualization pane
        2. drag the `specialty_Name` and `Avg/Med Wait List` Measure to the fields well.
        3. go to the Filters pane and select TOPN in the filter type of Filters on this visual
        4. select top 3 and drag `Avg/Med Wait List` Measure into the by value fields.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/51355886-d362-4273-acd2-7b25ea886ccd)

10. Create a Monthty trend visual for Total Wait List involving only  **Day case** and **Inpatients** using the Line Chart visualization. This is achieved by the following Steps.

        1. select the Line Chart from the visualization pane
        2. drag the Sum of `All_Data`[Total]  to the fields well y-axis 
        3. drag the `All_Data[Archive_Date] to the fields well x-axis
        4. drag the `All_Data[Case_Type] to the fields well legend
        5. Go to the Filter pane and under Filter on this visual, click Case_Type to select
         Basic Filtering,  select all the boxes bar Outpatient.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/b6e25126-dd8d-4963-a344-3ce043aaf65c)

11. Create a Monthty trend visual for Total Wait List involving only **Outpatient** using the Line Chart visualization. 

Repeat the steps in No.10 
select only the Outpatient box.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/b4388d9f-abbb-4446-88a4-69e552a23dfb)


12. Create a dynamic Title Card visual using the Card visualization to tell users which metric between `Average` and `Median` is selected at every piont in time. This is achieved by the following Steps.

        1. select the Slicer from the visualization pane
        2. drag the Latest Dynamic Tile Measure to the fields well 

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/6ef2ffa5-ad4a-4ed8-998a-d5e3e0483962)

13. Disconnect these viusals   
*  Monthty trend visual for Total Wait List involving only  **Day case** and **Inpatients**
*  Monthty trend visual for Total Wait List involving only **Outpatient**
from the `Case_Type` Slicer beacuse it has been filtered to perform specific task already. This is achieved by performing the following procedures

        1. select the Case_Type` Slicer and select Format
        2. select Edit Interactions
        3. toggle the symbol at the Header icon  the table to None
        4. unselect Edit Interactions



14. Create a Page Title: Summary using the Text Box 

#### **Step 4: Create visualizations Page 2(Detailed)**



1. Create a New page by clicking the plus **+** in the page pane
2. Rename the Page to Detailed
3. format the report Canvas Background to Black
4. create Slicers for this following and arrange them vertically
5. Sync the Slicers with the 1st page(Summary) using Sync Slicers
* `Archive_Date`
* `Case_Type`
* `Specialty_Name`
* `Age_Profile`
* `Time_Bands`

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/c495b3af-0cda-4560-b87e-a8df901aebbe)

6. Create a Matrix that summarizes our data and enable granular level analysis.

        3. select Matrix from the visualization pane
        4. drag  [Archive_Date], [Specialty_Name], [Age_Profile],[Time_Bands] to the fields well Rows
        5. 4. drag  [Case_Type]  & [Sum Total] to the  fields well Columns
        6. drag the Product[Category] to the fields well Drill through.

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/70c2928d-c23b-471b-8ee7-2853d9886f0a)

7. Create a Text Box with the Inscription **Detailed View**

8. Create a page Navigation button to switch over to the detailed page. 

        1. in the Summary page, Go to the insert tab and select Buttons, select the information button
        2. Go to the Format Buttons in the visualization pane and toggle Action to on.
        3. Under the Type dropdown, select page Navigation and select Detailed under Destination.
        4. Go to Tooltip and in the Text Field,type what will pop up when a cursor is hovered on the button

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/3a6d2af7-fab7-4011-9a71-4f5d0f671ecb)


9. Create a page Navigation Button also from Detailed View to the Summary View

        1. in the Summary page, Go to the insert tab and select Buttons, select the Back button
        2. this is already formatted to perform a backwards page Navigation

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/7075ed7c-a199-4ac4-a411-ad6fc8ccbcf6)

#### **Step 4: Create visualizations Page 2(Detailed)**

1. Create a Specialty group Customized tooltip visual using the Bar Chart visualization which shows a Bar visual when the cursor is hovered over the Monthty trend visual for both Day Case/Inpatient and Outpatient. This is achieved by the following Steps.

        1. Create a new page and rename it Drilldown
        2. format the report Canvas setting to custom:
        Height: 620px
        width: 320px 
        3. select the Bar Chart from the visualization pane
        4. drag the `Mapping_Specialty`[Specialty_Group] to the fields well y-axis 
        5. drag the All_Data[Sum of Total] to the fields well x-axis
        6. Create a All_Data[Sum of Total] sales card visual to highlight the Total Wait List 

2. Go to the Summary page and select the `Monthty trend Analysis visual for Day case and Inpatients` and Navigate to the Format, In the General tab perform the following steps
        
         1. Type: Report page
         2. page: Tooltip

![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/7923ca52-b772-4690-984b-0ccff4d89faf)

## Stage 3 Upload to Power BI Service:

 1. Save the Project in power Bi Desktop and select Publish

 2. choose the Workspace to Publish the report


![image](https://github.com/nnannaeze/Sales-Team-Quarterly-Report/assets/148848746/922d9d11-d04a-4dd7-81a7-faabecfb2956)


## Stage 4: Insights

The following inferences where drawn from this Report;
### [1] Latest Month Wait List 


a) Latest Month Wait List = 708,729

b) Previous Year  Latest Month Wait List = 640,441

There is a 9.64% increase in the Wait List for the Latest Month Wait List against the Previous Year  Latest Month Wait List


### [2] Average Wait List Case Type

The Average Wait List consists of the following Case Type categories in percentage 

* Outpatient = 72.49%
* Inpatient = 16.89%
* Day Case = 10.62%


### [3] Average Wait List with respect to Time Band & Age Profile

The Average Wait List for Time Band **18 Months +**, **0-3 Months**, **3-6 Months** accounts for the top 3 Average Wait List with the following values and Age Profile segments

a) for **18 Months +** = 307

        i.   16-64 years = 136
        ii.  0-15 years = 101
        iii. 65+ years = 66 

b) for **0-3 Months** = 207

        i.   16-64 years = 107
        ii.  0-15 years = 49
        iii. 65+ years = 51

b) for **3-6 Months** = 149

        i.   16-64 years = 74
        ii.  0-15 years = 40
        iii. 65+ years = 35

While the **15-18 months** accounts for the least Average Wait List with the following values and Age Profile segments

a) for **15-18 Months** = 90

        i.   16-64 years = 40
        ii.  0-15 years = 30
        iii. 65+ years = 20


### [4] Median Wait List with respect to Specialty_Name
the following Specialty has the top 5 Median Wait List 

* Accident and Emergency = 70
* Dermatology = 35
* Clinical (Medical) Genetics = 29
* Cardiology =  28
* Pain Relief = 27


### [5] Monthty Trend Analysis
The monthly Trend Analysis for **Day Case** and **Inpatient** is more of a uniform slope with the 
* minimum value = *67k* in month of February, 2020
* Maximum value = *86k* in the month of May, 2020

While he monthly Trend Analysis for **Outpatient** is more of an upward(Positive) slope with the 
* minimum value = *502k* in month of January, 2018
* Maximum value = *629k* in the month of March, 2021


### [6] Wait List by Specialty Group
The following Specialty Group accounts for the top 5 of Total Wait List for **Day Case** and **Inpatient** for the current month 
* General
* Bones 
* Urine
* Eyes 
* Heart

While the following Specialty Group accounts for the top 5 of Total Wait List for **Outpatient** for the current month 
* Bones
* General 
* ENT
* Skin
* Heart
