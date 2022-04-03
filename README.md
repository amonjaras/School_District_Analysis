# **SCHOOL DISTRICT ANALYSIS**

## **INDEX**

[1. Overview](#1-overview)

[2. Resources](#2-resources)

[3. Results](#3-results)

[4. Summary](#4-summary)


## **1. OVERVIEW**
Through out this Project, we support Maria, chief Data Scientist, for City School District, to provide a high level snapshot of each school district's key metrics to the School Board.

We analyze each school performance, based on the following:
- Students math and reading scores
- Students budget
- Type of School
- Size of School

The results of the analysis helped the School Board at state and district levels, to perform strategic decisions regarding budget allocations.

After the analysis sent to the School Board, they alerted Maria about a possible academic dishonest at Thomas High School, related to math and reading scores. Maria has requested us to replace the data and repeat the analysis.

Once data is replaced we will repeat the school district analysis and provide the comparison within this report.

[:arrow_up: Go to Top](#index)

## **2. RESOURCES**
Data provided to perform the analysis.
- [District School Data](https://github.com/amonjaras/School_District_Analysis/blob/main/Resources/schools_complete.csv)
- [District Student Data](https://github.com/amonjaras/School_District_Analysis/blob/main/Resources/students_complete.csv)

[:arrow_up: Go to Top](#index)

## **3. RESULTS**
As requested by Maria, we will replace the 9th grade reading and math scores by NaNs without affecting the rest of the results.

<details><summary><b>:arrow_left: press to access the explanation</b></summary>
<p>

### **3.1 Replacing 9th grade reading and math scores**
Utilizing the *loc* method we were able to select the reading and math scores from the 9th grade at Thomas High School, and replace them with NaN using the following code.

<details><summary><b>:computer: Code used for replacement</b></summary>
<p>

```
# Reading score
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") &
                    (student_data_df["grade"] == "9th"), "reading_score"] = np.nan

# Math score
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") &
                                     (student_data_df["grade"] == "9th"), "math_score"] = np.nan

# Checking the Results
student_data_df[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th")]
```
</p>
</details>

![9th grade replacement](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/ninth_replacement.png)

### **3.2 Repeat Analysis**
To have accurate results, we divided the analysis in the following sections:

#### **3.2.1 District Summary**
To stat our analysis we combine our data sources into a single dataset.
After the combination, this is the relevant information obtained in this section.

| | Student Count |
| :---: | :---: |
| Total | **38,709** |
> *Table 1: Total Student Count excluding 9th Grade for THS*

| Subject | Passing Percentage |
| :---: | :---: |
| Math | **74.76%** |
| Reading | **85.66%** |
| Overall | **64.86%** |
> *Table 2: Student Passing Percentages*

<details><summary><b>:computer: Part of the code used in this section</b></summary>
<p>

```
# Step 1. Get the number of students that are in ninth grade at Thomas High School.
thomas_student_count = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") &
                                                   (school_data_complete_df["grade"] == "9th"), "Student ID"].count()

# Calculate passing percentages
passing_math_percentage = passing_math_count / float(total_student_count_new) * 100
```
</p>
</details>

![District Summary](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/district_summary.png)


#### **3.2.2 School Summary**
During this section we started with the following summary per school

![School Summary1](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/school_summary1.png)

In order to get more accurate results it is important to calculate the passing criteria for THS grades 10th to 12th

| Subject | THS Passing Percentage |
| :---: | :---: |
| Math | **93.19%**|
| Reading | **97.02%** |
| Overall | **90.63** |
> *Table 3: THS grades 10th to 12th Passing Percentages*

Once obtained the results, it was necessary to replace passing results in the *per_school_summary_df*

![School Summary2](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/school_summary2.png)

<details><summary><b>:computer: Part of the code used in this section</b></summary>
<p>

```
# Step 6. Get all the students passing math from THS
ths_passing_math = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") &
                                              (school_data_complete_df["math_score"] >= 70)]
ths_passing_math_df = pd.DataFrame(ths_passing_math)
ths_passing_math_count = ths_passing_math_df.count()

# Step 9. Calculate the percentage of 10th-12th grade students passing math from Thomas High School.
ths_passing_math_percentage = ths_passing_math_count["Student ID"] / float(ths_student_count) * 100

ths_passing_math_percentage

# Step 13. Replace the passing reading percentage for Thomas High School in the per_school_summary_df.
per_school_summary_df.loc["Thomas High School", "% Passing Reading"] = ths_passing_reading_percentage
```
</p>
</details>

#### **3.2.3 High and Low Performing Schools**
With the new calculations these are the Top High and Top Low Performing Schools

> *Summary Top High Performing Schools*

![Top High](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/top_high.png)

> *Summary Top Low Performing Schools*

![Top Low](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/top_low.png)

#### **3.2.4 Math and Reading Scores by Grade**
To obtain the requested, we create a series of scores by grade levels.
The following images display the results obtained. Since we removed the scores of 9th grade for THS, the score is displayed as *nan*

> *Math scores by grade*

![Math Scores](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/math_scores_grade.png)

> *Reading scores by grade*

![Reading Scores](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/reading_scores_grade.png)


#### **3.2.5 Scores by School Spending**
For this section we created 4 bins to group the budget spend per student.
The spending bins were created as indicated on the next code;

```
all_spend_bins = [0, 585, 630, 645, 675]
group_names = ["<$586", "$586-630", "$631-645", "$646-675"]
per_school_summary_original_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, all_spending_bins, labels=group_names)
```

The spending ranges per student, are displayed in the following image.

![Spending Ranges](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/avr_spend_student.png)

#### **3.2.6 Scores by School Size**
For this section we created 3 bins to group the school size, based on the total students.
The size bins are indicated on the next code;

```
all_size_bins = [0, 999, 1999, 5000]
group_names = ["Small (<1000)", "Medium (1000-1999)", "Large (2000-5000)"]
per_school_summary_original_df["School Size"] = pd.cut(per_school_summary_original_df["Total Students"], all_size_bins, labels = group_names)
```

The size ranges are displayed in the following image.

![School Size](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/school_size_score.png)


#### **3.2.7 Scores by School Type**
Lastly we create two different groups related to the school type:
- District
- Charter

The following image shows the scores by school type;

![School Type](https://github.com/amonjaras/School_District_Analysis/blob/main/Images/school_type_score.png)

</p>
</details>


[:arrow_up: Go to Top](#index)

### **4. SUMMARY**
We can highlight the following information, for better understanding results after the *vs.* are considered after removing the scores for 9th grade at THS.

- **District Summary**
 - Total students and Total budget is the same. :white_check_mark:
 - Average math score dropped from 79 vs :small_red_triangle_down: 78.9
 - Average reading score remained the same :white_check_mark:
 - % Passing Math dropped from 75% vs :small_red_triangle_down: 74.8%
 - % Passing Reading dropped from 86% vs :small_red_triangle_down: 85.7%
 - % Overall Passing dropped from 65% vs :small_red_triangle_down: 64.9%

- **School Summary**
 - The new total student count is **38,709**
 - % Passing Math dropped from 93.27% vs :small_red_triangle_down: 66.91% for THS
 - % Passing Reading dropped from 97.31% vs :small_red_triangle_down: 69.66% for THS
 - % Overall Passing dropped from 90.95% vs :small_red_triangle_down: 65.08% for THS

- **Top Low and High scores**
 - For the Top High we can appreciate that THS has been dropped :small_red_triangle_down: from second place and replaced by Griffin High School.
 - For the Top Low we don't see any changes :white_check_mark:, meaning that THS wasn't affected by removing the 9th graders scores.

- **Scores by school spending**
 - % Passing Math dropped from 73% vs :small_red_triangle_down: 67%
 - % Passing Reading dropped from 84% vs :small_red_triangle_down: 77%
 - % Overall passing dropped from 63% vs :small_red_triangle_down: 56%

- **Scores by school size**
 - % Passing Math dropped from 94% vs :small_red_triangle_down: 88%
 - % Passing Reading dropped from 97% vs :small_red_triangle_down: 91%
 - % Overall Passing dropped from 91% vs :small_red_triangle_down: 85%

- **Scores by School Type**
 - % Passing Math dropped from 94% vs :small_red_triangle_down: 90%
 - % Passing Reading dropped from 97% vs :small_red_triangle_down: 93%
 - % Overall Passing dropped from 90% vs :small_red_triangle_down: 87%

Since THS is a Charter school, we appreciate a decrease in performance for Charter Schools.

In terms of School Budget per student, the analysis reflect that schools with lowest budget per student, have the highest overall performance on average.

[:arrow_up: Go to Top](#index)

  This work belongs to [^1].
  Course [^2].
  [^note]:
  [^1]: Author: Audrey MONJARAS :mexico: :canada:
  [^2]: Data Analytics: Unit 4 Pandas :snake: :panda_face:
