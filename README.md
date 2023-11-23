## ONLINE SQL-EDITOR

 This is an online SQL editor built especially for the frontend task of Atlan's front-end internship. This particular project is built using Svelte, and SCSS. 

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


The JavaScript framework I chose was SVELTE as it is better for smaller, more dynamic projects. Svelte is considered one of the fastest JavaScript frameworks and it is faster than React. It uses a compiler that does most of the work during build time, so there's no need to use a virtual DOM. Svelte shifts much of the work from the browser to the build process. This means that the framework itself is not included in the final bundle sent to the browser, resulting in smaller and more efficient runtime code.
 
The Major plugins or packages installed are gridjs, gridjs-svelte, papaparse, to-json-schema.

### Performance Audit

<img width="406" alt="image" src="https://github.com/madhusriram012/atlan_sql_front/assets/75003175/8e004718-628c-436e-891a-fc04ad6eec70">


### Brainstorming

For day-to-day life, data analytics might require,
1. Query execution and result visualization
2. Query history and version control
3. Performance tuning suggestion
4. Security and access control
5. Collaboration and sharing. With these initial thoughts I started implementing the features

### Features

1. Tab Based Interface: An easy-to-use tab based interface allows the user to switch between multiple queries at once. Each tab maintains its own separate state, so as long as you don't reload the page, you can jump right back to where you left a tab.

2. Dynamic Table Views: The list of tables is fetched at first, but the actual data isn't. Only when you click on the name of a table, are the entries fetched. Keeping the application lightweight, and swift.

3. Result Table Features: You can sort the data in alphabetical or in ascending order and the vice versa by just clicking the up and down arrows and one can view the schema of a particular table. The cool feature is that one can search in the table using the input box which fetches the required text instantly.It also shows how many rows are being displayed.

4. Result Statistics: The user will also be alerted about the time taken to complete a query, giving the user a measure to check the performance of the system.
Ability to save the results as JSON, XML, or CSV: This application includes functionality to save the results of a query in JSON, XML, and CSV formats. 


### Optimisations

1. Used pagination method to render a large amount of rows in application without breaking the browser, or without crashing it and using this method it provides the results very fast.
2. The most time-saving optimisation would be dynamic fetching. The rows of a table are fetched only when the user requests it. This saves a lot of seconds off our initial load time, by distributing that across requests.
3. Svelte itself is faster in initial load times and tends to have a more responsive user interface than most of the frameworks like React.


