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

### **Replacing 9th grade reading and math scores**
Utilizing the *loc* method we were able to select the reading and math scores from the 9th grade at Thomas High School, and replace them with NaN using the following code.

<details><summary><b>Code used for replacement</b></summary>
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

[:arrow_up: Go to Top](#index)

Here is the list of deliverables for the analysis of the school district:

- A high-level snapshot of the district's key metrics, presented in a table format
- An overview of the key metrics for each school, presented in a table format
- Tables presenting each of the following metrics:
  - Top 5 and bottom 5 performing schools, based on the overall passing rate
  - The average math score received by students in each grade level at each school
  - The average reading score received by students in each grade level at each school
  - School performance based on the budget per student
  - School performance based on the school size
  - School performance based on the type of school


  This work belongs to [^1].
  Course [^2].
  [^note]:
  [^1]: Author: Audrey MONJARAS :mexico: :canada:
  [^2]: Data Analytics: Unit 4 Pandas :snake: :panda_face:
