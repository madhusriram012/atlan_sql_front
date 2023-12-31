## ONLINE SQL-EDITOR

 This is an online SQL editor built especially for the frontend task of Atlan's front-end internship. This particular project is built using Svelte, and SCSS. 
 NOTE: Addition to using components, I'm using modularized class names such as FancyInput, FancySelect, and so forth for styling.

### How to run

### Step 1: Clone the repository

```bash
 git clone https://github.com/madhusriram012/atlan_sql_front.git
```
### Step 2: Install dependencies

To add node_modules, type npm install in terminal.
```bash
cd atlan_sql_front
npm install 
```
If error persists while running the above command ,simply delete the node_modules folder and repeat the process.

### Step 4: To start server

```bash
  npm run dev
```
After running the above command, open [http://localhost:5173](http://localhost:5173/) to view it in your browser.


### Framework

The JavaScript framework I chose was SVELTE as it is better for smaller, more dynamic projects. Svelte is considered one of the fastest JavaScript frameworks and it is faster than React. It uses a compiler that does most of the work during build time, so there's no need to use a virtual DOM. Svelte shifts much of the work from the browser to the build process. This means that the framework itself is not included in the final bundle sent to the browser, resulting in smaller and more efficient runtime code.
 
The Major plugins or packages installed are gridjs, gridjs-svelte, papaparse, to-json-schema.

### Demo

[DEMO_FRONTEND_ATLAN](https://drive.google.com/file/d/16U8vS3gwn7VB3tA4CGdb4qpdI_hjLjQv/view?usp=sharing)


### Performance Audit

Websites used to check : 1. [GTMetrix](https://gtmetrix.com/reports/madhu-atlan-sql-editor.netlify.app/ulYJ3Xg9/) , 2. [WebPageTest](https://www.webpagetest.org/), 3.[LightHouse](https://pagespeed.web.dev/)

## Before Improvements

<img width="846" alt="image" src="https://github.com/madhusriram012/atlan_sql_front/assets/75003175/de4cc81c-fded-42ae-8aa1-7fd4853825bb">


## After Improvements

<img width="1220" alt="image" src="https://github.com/madhusriram012/atlan_sql_front/assets/75003175/b85176b5-53f9-44b0-a7f2-570a0cd90d73">


<img width="606" alt="image" src="https://github.com/madhusriram012/atlan_sql_front/assets/75003175/76574ec6-5ba2-4063-b658-4722a4c737dc">



### Brainstorming

Brainstorming started with gathering necessary information required for the data analystics team to have smooth expereince by using the tool.
With the gathered information, few important topics were picked and features were built based on those needs. The following are the topics the features were focussed on,
* Query execution and result visualization
* Query history
* Performance tuning suggestion
* Security and access control
* Collaboration and sharing.


### Features

1. Tab Based Interface (Query execution and result visualization): Tab based interface allows the user to switch between multiple queries at once. Helps to improve their speed and keep different queries seperately in case if they need to switch. 
2. Dynamic Table Views (Query execution and result visualization): The list of tables will be fetched without loading all the data. Only when the user selects the name of a table, the results will be loaded in pagination view. Keeping the application lightweight, and swift.
3. Result Table Features (Query execution and result visualization): This gives flexibility for the user to do actions on the fetched results. The user can sort/search/move to different set of results using pagination.
4. Search Tables (Query execution and result visualization): The user can search for the table names. In real production environment, there will be lot of tables which needs to be searched. This feature saves time by identifying required table faster.
5. History  (Query History): History tab shows the list of queries which was used. 
6. Dedicated account (Security and access control): The user will have dedicated account which allows to authenticate and authorise user. Some users can access certain table and some can't. This account helps to manage these restrictions.
7. Sharing (Collaboration and sharing): Users can share the query to the users in the organisation or the team. 
8. Download (Collaboration and sharing): User can download the result in different formats (CSV, Json , XML) for future use or also for sharing.
9. Result Statistics (Performance tuning suggestion): The statics about the number of rows and the time taken were displayed. This helps to tune the queires to have better performance. 

### Few more add-ons:

1. Saved Option - This helps the user to save the report/query which can be used later. 
2. Schema - The result table also shows the schema of the table which will be useful to identify the types of the columns.
3. Environments - The user can switch between different environments. As in real time, the user will have multiple environments to work on.


### Optimizations

1. Used pagination method to render a large amount of rows in application without breaking the browser, or without crashing it and using this method it provides the results very fast.

2. The most time-saving optimisation would be dynamic fetching. The rows of a table are fetched only when the user requests it. This saves a lot of seconds off our initial load time, 
   by distributing that across requests.
   
3. Svelte itself is faster in initial load times and tends to have a more responsive user interface than most of the frameworks like React.
   
4. Image was fetched directly from a website and lazy loading was enabled so it increased the load time so added the image along with the code in local and removed lazy loading as the 
   image comes in the main page.

5. Did cache Busting in order to change the URL of the static files whenever one make updates, forcing the browser to fetch the new version.

