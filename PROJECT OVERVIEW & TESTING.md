**Database Structure**
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This integrated mineral intelligence platform connects geologists, surveyors, mining companies, investors, and government agencies while automating mineral exploration reporting, geological mapping. Real-time data on mineral type, quality, quantity, and regional demand enables informed investment recommendations, reduces exploration uncertainty, and accelerates the movement of mineral resources from discovery to market.

***Pluggable database created***

<img width="1320" height="702" alt="final1" src="https://github.com/user-attachments/assets/a159607b-9cdd-43b6-84d1-d292f7d5bfa0" />

***Regions***

*Purpose: Acts as the top-level geographical container.

*Role: Defines the broad areas where survey activities take place. It stores descriptive data about the territory (like general location and characteristics) which helps in categorizing sites.

<img width="512" height="288" alt="region" src="https://github.com/user-attachments/assets/10c8f620-46f0-4406-8ea6-9826001946b2" />

***Minerals***

*Purpose: A reference catalog (lookup table) for geological materials.

*Role: Stores the names and categories of all possible minerals that could be discovered. By keeping this separate, the system ensures data consistency (e.g., preventing different spellings of "Quartz").

<img width="1038" height="452" alt="minerals" src="https://github.com/user-attachments/assets/11026e6d-bee5-443a-aa2d-c4870eb34f87" />

***Sites***

*Purpose: Defines specific points of interest within a region.

*Role: Links a physical location (Site) to a specific REGION. It tracks site-specific physical data, such as the maximum Depth reached during exploration.

<img width="1037" height="474" alt="site" src="https://github.com/user-attachments/assets/a375c579-babd-45e6-a464-e155f7566cc6" />

***Surveyor***

*Purpose: Manages the personnel profile.

*Role: Stores the identity and contact information of the professionals conducting the surveys. It ensures that every report generated can be traced back to a specific individual.

<img width="1037" height="470" alt="surveyor" src="https://github.com/user-attachments/assets/4bf7f2ea-c269-4eb7-bc8f-ec49fa6f80fe" />

***Findings***

*Purpose: Records the actual results of exploration at a site.

*Role: This is an associative table that connects SITES to MINERALS. It records what was found, how much (Quantity), and when (FindingDate). It is the most critical table for data analysis.

<img width="1035" height="510" alt="findings" src="https://github.com/user-attachments/assets/1920d42e-cab7-4cf3-b949-a40a850fb517" />


***Report***

*Purpose: Summarizes site activities and professional observations.

*Role: Connects a SURVEYOR to a SITE. While FINDINGS tracks raw data (numbers and dates), the REPORT table stores qualitative data (using the CLOB Summary field) for detailed professional documentation.

<img width="1038" height="587" alt="report" src="https://github.com/user-attachments/assets/b7967c5c-5074-43d9-847e-71a9e477cb3b" />

***ER Diagram***

The ER diagram serves as a visual blueprint for a Geological Survey and Mineral Tracking System. It organizes data into a structured hierarchy that ensures data integrity while allowing for complex reporting on mining activities.

Here is a breakdown of the diagram’s structure and flow:

1. The Geographical Hierarchy
The relationship starts at the REGION level. This is a one-to-many relationship with SITES, meaning one broad geographical area can contain many specific excavation points. This structure allows management to filter mineral data by general location or specific site depth.

2. The Discovery Engine (Findings)
At the heart of the diagram is the FINDINGS entity. This acts as a "junction" between SITES and MINERALS.

It resolves the relationship of "What was found where?"

By linking these, the system can track the exact quantity and date a specific mineral was extracted from a specific site without duplicating mineral names or site details.

3. Professional Accountability (Reports)
The SURVEYOR and REPORT entities represent the human element of the data.

A SURVEYOR is linked to a REPORT as the author.

Each REPORT is then linked back to a SITE. This creates a clear "paper trail," ensuring that every professional summary or site evaluation is tied to both the expert who wrote it and the location they were inspecting.

***Logical Flow Summary***
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Locate: Identify the Region and Site.

Identify: Record the Mineral discovered via a Finding.

Document: The Surveyor files a Report on that site's progress.

image

**TEST Result**
---------------------------------------------------------------------------------------------------------------------------------------------------

***TEST 1: Validation Function (TEST via logic)***

*What it does: Uses a CASE statement to perform a conditional check on a value.

*Purpose: This is a "sanity check" often used during data insertion. It ensures that data—specifically the Quantity field—meets business requirements (e.g., you cannot find a negative amount of a mineral) before it is permanently saved to the database.

<img width="1032" height="580" alt="validation" src="https://github.com/user-attachments/assets/fd654976-b2fb-4871-a9f4-1dfd8a33d5ca" />

***TEST 2: Lookup Function***

*What it does: Retrieves specific attributes (MineralName) based on a unique identifier (Mineral_id).

*Purpose: This demonstrates the efficiency of your relational design. Instead of typing "Gold" manually every time, the system "looks up" the name from the MINERALS table. This prevents typos and ensures that the ID 1 always refers to the same mineral across all findings.

<img width="1037" height="591" alt="function test" src="https://github.com/user-attachments/assets/4f54d7f8-3148-410d-b824-1aa9f000cdd0" />

***TEST 3: Aggregate with OVER Clause***

*What it does: Calculates a running or partitioned total without collapsing rows.

*Purpose: Unlike a standard SUM which gives you one single total, this allows you to see individual Quantity entries side-by-side with the Site Total. It is extremely useful for reporting, as it shows how much a specific finding contributes to the overall output of a mining site.

<img width="1037" height="594" alt="quantity found" src="https://github.com/user-attachments/assets/0c1ba286-6baa-45e8-949b-a693844ff2d6" />

***TEST 4:EXCEPTION HANDLING testing***

*What it does: Catches a standard Oracle error that occurs when a SELECT statement returns zero rows.

*Purpose: This prevents the entire application from crashing if someone searches for a Mineral ID that doesn't exist (like ID 999). Instead of an "Ugly" system error, it provides a clean, readable message: "Mineral not found."

<img width="1038" height="452" alt="procedure" src="https://github.com/user-attachments/assets/9da05d8d-ec29-47c9-b829-b84afe0ddcab" />

***TEST 5: Custom Exception testing***

*What it does: Defines a specific business rule error (e_invalid_qty) that isn't built into the database by default.

*Purpose: This enforces high-level business logic. While the database might technically allow a negative number in a numeric field, this code forces the system to "Raise" an error if the quantity is zero or less, ensuring your geological data remains physically accurate.

<img width="1037" height="563" alt="custom function" src="https://github.com/user-attachments/assets/c42987f7-7685-437f-be99-5cb89548f35a" />




