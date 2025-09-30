# SECTION 12. Introduction to Shiny



In this lesson, we will work on the creation of a Shiny application to visualize specific data. The app will include interactive charts and styled data table. By following these detailed steps, we will set up and deploy a Shiny app on Shinyapps.io.

Shiny and other related packages can be installed from CRAN. Let's see the main functionalitites of the shiny-related packages:

-   `shiny` provides the core functionality for building interactive web apps.

-   `shinydashboard` offers tools for creating dashboards with Shiny.

-   `plotly` converts `ggplot2` graphics to interactive plots.

-   `DT` renders interactive data tables.

## Writing a Shiny Application in R

Shiny applications have two components, a **user** interface object and a **server** function. Both components are passed as argument to the `shinyApp` function that created the app as a result of this UI/Server pair.

### UI Layout

The user interface (UI) defines how users interact with the application. We define the structure of the app to include tables and graphics and drop-down menus for selecting different parameters. This allows users to dynamically interact with the data.

### Server Logic

The server function processes user inputs and updates outputs.

### Examples

All the examples are extracted from <https://shiny.posit.co/r/articles/start/basics/>.

**Application Shiny 1**: we plot a random distribution as a histogram with a requested number of bins.

In the UI component we define the aspect/aesthetics of the application. In the server section, we define the steps needed to display the graphic we aim to plot. We use different elements to define both parts that will be discussed below. Try to execute all these lines and check the output after running the last line `shinyApp(ui, server)`.



**Application Shiny 2**:



## Elements for a Shiny App

### UI Components

Check list of inputs: https://shiny.posit.co/r/components/ 

Check list of outpus (section Outputs): https://shiny.posit.co/r/components/ 


| **Function**       | **Type**        | **Description**                                                                                     | **Example Use Case**                                                                                   |
|---------------------|-----------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `textInput`         | Text Box        | A single-line input field where users can enter text.                                               | Collecting a user’s name or entering a search term.                                                   |
| `sliderInput`       | Slider          | Allows the user to select a numeric value or range by sliding a handle.                             | Setting a range for filtering data (e.g., selecting years or numeric thresholds).                     |
| `selectInput`       | Dropdown Menu   | Provides a dropdown menu for choosing one or multiple options.                                      | Selecting a category from a list of items (e.g., region, product, or variable).                       |
| `checkboxInput`     | Checkbox        | A simple checkbox for toggling between TRUE (checked) and FALSE (unchecked).                        | Turning features on/off in a plot (e.g., showing gridlines or enabling normalization).                |
| `radioButtons`      | Radio Buttons   | A group of mutually exclusive choices displayed as a list of buttons.                               | Selecting a chart type (e.g., bar chart, scatter plot, or line graph).                                |
| `fileInput`         | File Upload     | Allows the user to upload a file to the server.                                                     | Uploading a CSV file for analysis.                                                                    |
| `numericInput`      | Numeric Input   | Input box for entering a numeric value.                                                             | Setting the sample size for simulation.                                                               |
| `dateInput`         | Date Picker     | Provides a calendar for selecting a single date.                                                    | Selecting a start or end date for time-series data.                                                   |



| **Function**       | **Type**         | **Description**                                                                                     | **Example Use Case**                                                                                   |
|---------------------|------------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `textOutput`        | Text             | Displays reactive text or plain text generated in the server.                                       | Showing user selections or calculated results.                                                        |
| `plotOutput`        | Plot             | Displays plots (e.g., ggplot2 or base R) created in the server.                                     | Visualizing distributions, time series, or trends in data.                                            |
| `tableOutput`       | Table            | Displays a static data table generated by R.                                                        | Showing a summary table of aggregated data.                                                           |
| `DT::dataTableOutput`| Interactive Table| Displays an interactive data table with sorting, filtering, and pagination (requires the `DT` package).| Allowing users to explore a dataset interactively.                                                    |
| `imageOutput`       | Image            | Displays static or dynamically generated images.                                                    | Displaying uploaded or generated images (e.g., maps or diagrams).                                     |
| `uiOutput`          | Dynamic UI       | Dynamically renders UI components in the server.                                                    | Generating custom input fields based on user choices.                                                 |
| `verbatimTextOutput`| Verbatim Text    | Displays unformatted text, often used for showing code or debugging output.                         | Printing console output for diagnostics or displaying R expressions.                                  |




### Server function


| **Function**           | **Description**                                                                                     | **Example Use Case**                                                                                   |
|-------------------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `input$<inputId>`       | Accesses the value of an input element specified by its `inputId`.                                  | Retrieve the value of a slider (`input$slider1`) or a dropdown menu (`input$dropdown1`).               |
| `observe`               | Observes changes to inputs without returning a value (used for side effects).                       | Updating a log file or triggering animations when a button is clicked.                                |
| `reactive`              | Creates a reactive expression that can depend on inputs or other reactives.                        | Defining a filtered dataset based on user selections.                                                 |
| `observeEvent`          | Observes a specific event (like a button click) and runs code in response.                         | Executing code only when a submit button is pressed.                                                  |
| `isolate`               | Accesses inputs without triggering reactivity.                                                     | Reading values of inputs without causing dependencies (e.g., snapshotting user input).                |


| **Function**           | **Output Type**      | **Description**                                                                                     | **Example Use Case**                                                                                   |
|-------------------------|----------------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `renderText`            | Text                | Generates text output for `textOutput` in the UI.                                                   | Showing calculated values or messages.                                                                |
| `renderPlot`            | Plot                | Generates a plot for `plotOutput` in the UI.                                                        | Visualizing distributions, trends, or comparisons.                                                    |
| `renderTable`           | Static Table        | Creates a table for `tableOutput` in the UI.                                                        | Displaying a data summary or descriptive statistics.                                                   |
| `DT::renderDataTable`   | Interactive Table   | Renders a table for `DT::dataTableOutput` in the UI (requires the `DT` package).                     | Enabling sorting, searching, and pagination of large datasets.                                        |
| `renderImage`           | Image               | Renders an image for `imageOutput` in the UI.                                                       | Displaying dynamically generated images (e.g., visual results of an algorithm).                       |
| `renderUI`              | Dynamic UI          | Dynamically creates UI components for `uiOutput` in the UI.                                          | Generating UI elements (e.g., a new slider) based on user input or external data.                     |
| `renderPrint`           | Verbatim Output     | Displays text or objects as they appear in the R console.                                            | Printing debugging information or showing raw data.                                                   |



## Working with ShinyDashboard

Here we will describe an example and go step by step to understand the code behind the application. We will work with the dataset `data_tidy.txt` that we have used in previous classes.

1- We read the dataset and inspect the elements:



We need to define what is our aim creating this application. Let's define 3 main goals:

1) To show a main page with the information reported in the App.

2) Display "X" lines of observations (interactive). 

3) To show a descriptive table only for specific cases of the dataset (interactive filter).

4) To display a graphic displaying the weight of the participants based on a specific factor (interactive input). 


2- We define the UI and server function of the application:

**UI section**

To start a dashboard page we need to define at first `dashboardPage` in the first line of code and then write all the content inside the page. We will have a main header for the Shiny App that will be defined using the `dashboardHeader` function. Then, to describe a sidebar with the main sections of the App, we need to work with the `dashboardSidebar`function. Inside this section we can customize our `sidebarMenu` by adding as many items as we want to the application (`menuItem`). 

Once we have defined the main tabs that the app will have, we can start defining the content within each one of these tabs by indicating everything within the section `dashboardBody`. The content will be splitted by `tabItem` sections within a general `tabItems` label. Each `tabItem` relates to the differnent `menuItems` that appear in the sidebar. All the interactive inputs and outputs are defined in this part of the code. The body of the `tabItem`can be divided into different spatial areas. The body can be treated as a region divided into 12 columns of equal width, and any number of rows, of variable height. When you place a box (or other item) in the grid, you can specify how many of the 12 columns you want it to occupy. 

Scheme:

- dashboardHeader

- dashboardSidebar > sidebarMenu > menuItem 

- dashboardBody > tabItems > tabItem > fluidRow > box

The aesthetics of a box can be modified, such as the color (status):

- "primary" (blue)

- "info" (light blue)

- "success" (green)

- "warning" (orange)

- "danger" (red)`.






**Server function**:




3- We run the application




Summary:








## Deploying the Application to Shinyapps.io

1.  Create a Shinyapps.io Account: <https://www.shinyapps.io/?ref=lorcanmason.com>

a- First, sign up:



b- Second, add a token:





2.  Configure deployment:

2.1 Configure your account: in Rstudio, load the rsconnect package and set your account information

a- `install.packages("rsconnect")`

b- `library(rsconnect)`

c- `rsconnect::setAccountInfo(name='YOUR_ACCOUNT_NAME', token='YOUR_TOKEN', secret='YOUR_SECRET')`: replace it with your Shinyapps.io credentials:

Go to your shinyapps.io profile and in the tab tokens, add a new one. Then press the button "Show" and "show secret" and copy the text. Run this line of code in R studio.




After deployment, Shinnyapps.io provides a URL where you can access your app

d- `rsconnect::deployApp('path/to/your/folder')`: replace it with the path to your app.R file to deploy your application. You need to indicate the path to the folder where the app.R file is stored.




In this last step you can obtain many messages of "Error" because of lack of compatibility of versions for specific packages. Update all the packages you need to update and install those that are also required.


Be aware that a large app or first-time server setup can take time (30 minutes) and while it is deploying to server, you may not be able to work in R.


## Take home

1.**Shiny Components**: Shiny core components are `ui`, `server`, and the `output` of the App. For more information about the Shiny app script check:

-   <https://shiny.posit.co/r/getstarted/shiny-basics/lesson1/>

2.**Customizing Visualization**: `ggplot2` and `plotly`.

-   More information about `plotly`: <https://plotly.com/r/?ref=lorcanmason.com>

-   More information about `DT`: <https://rstudio.github.io/DT/?ref=lorcanmason.com>

3.**Interactive Elements** : implementing user inputs and outputs(`render`) to build interactive applications.

4.**Deployment**: ensure your app is deployed for easy access and sharing. For more information about the dasbhoard:

-   <https://rstudio.github.io/shinydashboard/structure.html#background-shiny-and-html>


## Final Task

For this final assignment you will work with the excel file **dataADNIMERGE.xlsx**. This data comes from a multicentric study in Alzheimer's disease and contains longitudinal data for participants with different diagnosis. You need to subset the observations/cases reported at baseline (VISCODE variable) and work with a final dataset including the variables **ORIGPROT** (batch), **DX.bl** (diangosis baseline), **AGE** (age baseline), **PTGENDER** (Sex), **PTEDUCAT** (years of education), **APOE4** (genotype for APOE-e4, 0=non-carriers, 1=heterozygous for allele e4, 2=homozygous for allele e4), **ABETA.bl** (amyloid-beta levels at baseline),**PTAU.bl** (pTau levels at baseline), **MMSE.bl** (score in the MMSE test memory at baseline). 

The activity consists on:

**1) Creating a report**: create a `.html` file (markdown) and divide the content in two main sections. The **first section** needs to contain a brief summary of the data of the study (number of participants, proportion women/men, clinical diagnosis of the participants, mean age). You need to report the main summary statistics and explain the main characteristics of the sample. You can check the [ADNI webpage](https://adni.loni.usc.edu/) in case you want to retrieve more information. The **second section** relates to the shiny app creation. You need to explain the application that you have created, the main sections, the purpose of the visualization and the main elements you have used to define the user and server interfaces. You need to provide the link to access the shiny dashboard (Shinyapps.io). 


**2) Creating a shiny dashboard**: create a shiny app in R and deploy it to the server. The shiny app needs to contain:

1. One tab where you introduce the main functionality of the app (hint: `.md` file).

2. One tab including a static table.

3. One tab including a reactive table.

4. One tab including a static plot.

5. One tab including a reactive plot.


Submit all the files you have generated in a `.zip` folder. Remember, it should contain the `.html file` (report) and all the `R files` used to create the shiny app (`app.R` and `.md` files). The name of your `.zip folder` should be your surname (e.g. Genius.zip)





