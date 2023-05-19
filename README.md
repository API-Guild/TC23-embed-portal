# Build your own Analytical App

# NB This is a work in progress and will be updated ASAP to contain the correct images and links

Welcome to this hands-on-training session! 

We will cover the following today:
- Leverage Tableau's Embedding API v3 to interact with and embed visualizations & dashboards into a web application
- Configure a Connected App to enable single-sign-on authentication for external users
- Configure User Attribute Functions to pass in data authorizations at runtime

Plan on following along and attempting the exercises to get the most out of today's class.

## Agenda
**[Initial Setup](#initial-setup)**

**[Lesson 1: Simple Embedding](#lesson-1-simple-embedding)**

**[Lesson 2: Connected Apps](#lesson-2-connected-apps)**

**[Lesson 3: Advanced APIv3](#lesson-3-advanced-apiv3)**

**[Lesson 4: User Attribute Functions](#lesson-4-user-attribute-functions)**

**[Lesson 5: Complete Solution](#lesson-5-complete-solution)**

**[Solution Code Snippets](#solution-code-snippets)**

<br/>
Click on an Agenda item to navigate to the relevant section.

---

#### Additional links
- [Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html)
- [Connected Apps](https://help.tableau.com/current/online/en-us/connected_apps_direct.htm)
- [User Attribute Functions] (https://help.tableau.com/current/api/embedding_api/en-us/docs/embedding_api_user_attributes.html)
- [Tableau Embedding Playbook](https://tableau.github.io/embedding-playbook/)


# Initial Setup

## This will differ to the lab, as you will need to set up your environment manually in order to work through the course. 
> Here are the high level steps you need to take:

1. - [ ] Install Python
2. - [ ] pip install -r requirements.txt
3. - [ ] Install VSCode (or any other IDE you're familiar with) 
4. - [ ] Ensure you have a Tableau Cloud site where you are an admin (Get a new developer site if you want)
5. - [ ] Create a top level project in Tableau Cloud called "hotembed"
6. - [ ] Upload workbook_userxxx.twbx from resources folder into project hotembed renaming it "workbook_hotembed"
7. - [ ] Upload redirect_userxxx.twbx from resources folder into project hotembed renaming it "redirect_hotembed"
8. - [ ] Create a new viewer user, this can be any email you want to use, just remember it for logging in with CA later
9. - [ ] Change the variables at the top of EmbeddedPortal.py
10. - [ ] hotUser = "hotembed" in line 16
11. - [ ] Set the URL of your Tableau Cloud site and Site Name in lines 27 and 28
12. - [ ] Launch a terminal window and run EmbeddedPortal.py (The exact command may depend on which terminal used) 
13. - [ ] Luncnh a browser and point to "localhost"

If successful you should see the TC23 menu screen!!! 

## NB thoughout this course, where insturctions say log into Tableau, use your own user credentials you used to log into Tableau Cloud as an admin. Where the instructions are logging you into the portal using connected apps, use the new viewer user you set up in step 8 above. 
===

# Lesson 1: Simple Embedding

>Here are the high level steps you need to take:
1. [] Get familiar with Visual Studio Code
1. [] Get familiar with TC23 application
1. [] Complete exercise 1

<br/>

In the previous section, you logged into our Tableau Cloud Site to see where the dashboards are hosted and configured. Let's now pivot to the other two applications we'll be spending time in today.

---

###Step 1: Get familiar with VS Code

VS Code is where our application code is stored and where we'll manipulate the code in the exercises.

Switch to VS Code. You'll see all the relevant HTML & JavaScript files on the left hand side. They are numbered to coincide with the lesson/exercise we are on, and, specifically, the html & js files for lesson 1 have automatically been opened for you.

The web application we are using today to embed Tableau analytics into was made with Flask. This app (*EmbedPortal.py*) is already running on your machine, which you can see by looking at the active terminal window at the bottom of VS Code.

In the terminal, you'll see something like:

```127.0.0.1 - -  [timestamp] "GET /static/..." -```

<b>If your VS Code setup doesn't seem right or your web server isn't running correctly, please raise your hand and an assistant will come by.</b>

<br/>
###Step 2: Get familiar with TC23 application

In your non-incognito browser window, you should see a web page with five links to each of the lessons covered in this class. This is the TC23 web app we'll be embedding tableau dashboards into.

Click --> *Lesson 1: Simple Embedding*

![0E17B15C-6F71-4D9D-A08B-7A3871560F57.jpeg](0E17B15C-6F71-4D9D-A08B-7A3871560F57.jpeg)

You should now be in the app and see a Tableau Sign-in page embedded in the middle. This is what we call a 'Simple Embed' where there's no Single Sign-On component to this.. The user has authenticated into the TC23 Application successfully, but has not signed into Tableau yet.

Go ahead and click on the "Sign in to Tableau Cloud" and use your admin credentials to sign-in:

Username = +++@lab.CloudCredential(TC2023-HOT20).Username@embeddedhot.com+++

Password = +++Password1!+++

By signing in, you now have a session with Tableau and can interact with this visualization and others too. If you're curious, in this example, we use a SAML idenity provider to establish the session. In lesson 2, we'll learn how to do this sign-in behind the scenes for the end user so it feels seamless.

Now, notice the toolbar at the top of the dashboard and all of the buttons it includes. Just below the toolbar, use the REGION filter dropdown to switch the region twice (for ex, West & then East). Then use the first three buttons on the toolbar to Undo, Redo, and then Reset view.

Aside from the first three buttons, we don't need the rest of these. Let's learn how to remove the toolbar and wire up our custom html buttons on the left hand side to keep the functionality we do want.

<br/>

---
>[!note] ###Step 3: Complete exercise 1
For the first exercise, we're going to have you:
- Hide the Tableau toolbar from the dashboard
- Wire up the Revert to Original, Undo, and Redo buttons on the left-hand side

For this exercise, go to VS Code and look at the *lesson1.html* and *embed1.js* files

Lines of code to note in lesson1.html:
- Link to the Embedding API library (*Ctrl+F for 'module'*)
- 3 html buttons that will interact with the tableau dashboard (*Ctrl+F for 'onclick'*)
- Tableau viz web component with configuration parameters (*Ctrl+F for 'tabserver'*)

To remove the toolbar, [look at this doc](https://help.tableau.com/current/api/embedding_api/en-us/docs/embedding_api_basic.html) and make the relevant change to lesson1.html. (*Ctrl+F for 'toolbar' or 'hidden'*)

To wire up the Revert to Original, Undo, and Redo buttons, first you need to create the Viz object by grabbing the *tableauViz* div from the html document.

Then, [look at the Viz object methods here](https://help.tableau.com/current/api/embedding_api/en-us/reference/interfaces/vizactions.html) and make the relevant changes to embed1.js. (*Ctrl+F for redoAsync() & undoAsync() & revertAllAsync()*)

And don't forget those methods must be used on a Viz object.

When you're done making your changes to *lesson1.html* & *embed1.js*, save your changes and refresh your TC23 page in the browser. Check that the toolbar is hidden. And switch between a couple of Regions in the dropdown filter on the dashboard so that you can then test the buttons left of the dashboard.

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

===

#Lesson 2: Connected Apps

>Here are the high level steps you need to take:
1. [] Create a Connected App
1. [] Configure the JWT
1. [] Verify CA works
1. [] [OPTIONAL] Complete exercise 2

<br/>

In the previous section, you demonstrated a simple embed without Single-Sign-On. Let's now pivot to setting up SSO for seamless authentication for external users. We will do this via Connected Apps.

---

###Step 1: Create a Connected App
In your non-incognito browser, log out of the TC23 app --> Click 'Welcome!' in top right corner --> Click Logout

Click --> *Lesson 2: Connected Apps*

Notice that our TC23 app presents us with a sign-in page for this exercise. When we sign into the app, we should not have to sign-in to tableau again. To set this up, we need to configure a Connected App in Tableau Cloud.

Switch to your incognito browser and make sure you're logged into Tableau Cloud with your admin credentials:

Username = +++@lab.CloudCredential(TC2023-HOT20).Username@embeddedhot.com+++

Password = +++Password1!+++

Make sure you're on the landing page (Hit the back button if you're looking at a Dashboard --> Click Home)

We're looking for the external user associated with your lab --> Click on the Users tab --> Search for:

Username = +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++

**This is your external user credential (no password needed). Make note of it.**

Now onto configuring a Connected App --> Click on Settings tab --> Click on Connected Apps tab

Click New Connected App --> Click *Direct Trust* --> Choose the following settings:
- Connected app name = +++CA_@lab.CloudCredential(TC2023-HOT20).Username+++ (name has to be unique)
- Access level: Applies to = Only one project
- Access level: Project name = +++@lab.CloudCredential(TC2023-HOT20).Username+++  (<u>Very important to pick only your project!</u>)
- Domain allowlist: All domains
- Click Create

Click Generate New Secret

Click 3 dots next to Connected App Name --> Enable

Below the name, you should see Status <b>Enabled</b>

![E0461E2B-7B09-4A45-BBEF-7B550217A015.jpeg](E0461E2B-7B09-4A45-BBEF-7B550217A015.jpeg)

<br/>
###Step 2: Configure the JWT
After creating the CA in Tableau Cloud, we need to enable our app to send a valid JWT. JWT is a standard used to securely transfer information between two parties. The JWT is signed by our app to securely send info to Tableau Cloud.

In Visual Studio, open EmbedPortal.py --> Find the getJWT() function (*Ctrl+F for 'getJWT'*)

Copy Client ID from Tableau Cloud CA configuration page --> Replace 'connectedAppClientID' value (*2 times*)

Copy Secret ID --> Replace 'connectedAppSecretID' value (*1 time*)

Copy Secret Value --> Replace 'ConnectedAppSecretKey' (*above Algorithm*)

Save your changes to the EmbedPortal.py file. You should see the terminal update.

<br/>
###Step 3: Verify CA works
Now that we've configured our app to create a valid JWT, let's test it.

Switch to the TC23 app in your non-incognito browser tab --> Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++

If you configured everything correctly, you should see an intermediate page with a message: **Welcome @lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com, to Connected Apps** and an image.

Now click the Load Your Dashboard! button below the JWT token.

This will take you to the landing page in the app and the dashboard should load successfully. You will notice the logged-in-user's usersname in the top left corner of the dashboard. Also, notice that the user did not have to sign into Tableau after signing into the TC23 app.

 <br/>

 ---
>[!note]###[OPTIONAL] Complete exercise 2
For this exercise, you are going to:
- Switch out the dashboard that is embedded for a different dashboard
- Configure the CA to work with the new dashboard

To switch out the dashboard:
- In VS Code, take note of the dashboard URL in lesson1.html (*Ctrl+F for 'MyTCDash'*)
- In Tableau Cloud, log back in --> Click on Explore tab on left --> navigate to the *Samples* Project --> the Regional workbook --> the Global Temperatures dashboard
- Look for the same URL pattern you saw in lesson1.html and replace it in VS Code --> Save

You can test your changes by logging out of the app, clicking on Lesson 2, and going through the login process again.

You'll notice that the JWT seems to be created successfully, but the dashboard won't load.. Why? Is your JWT configured correctly? (*Hint: Tweak your CA configuration and then refresh the app to test*)

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

===

#Lesson 3: Advanced APIv3

>Here are the high level steps you need to take:
1. [] [OPTIONAL] Complete exercise 3.1
1. [] Complete exercise 3.2.1
1. [] [OPTIONAL] Complete exercise 3.2.2
1. [] [OPTIONAL] Complete exercise 3.3
1. [] Complete exercise 3.4

<br/>

In the previous section, you configured SSO for a simple embed. Let's now pivot to some more advanced features of the Embedding API. We don't have enough time to step through each exercise, so some will be marked as optional.

---
>[!note]###[OPTIONAL] Complete exercise 3.1
For this exercise, you are going to:
- Configure html buttons to switch between 3 different vizzes

To get started:
- Switch back to the TC23 app in your non-icognito browser window --> Logout
- Click --> *Lesson 3: Advanced API v3*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click *Lesson 3_1: Multi Viz* link
- Expand the *Select Viz* button on the left hand side. This reveals three individual buttons that should load different vizzes each. In this exercise, you will configure these buttons to work.

![1A77FC69-6910-463E-82BC-5720119BB48B.jpeg](1A77FC69-6910-463E-82BC-5720119BB48B.jpeg)

Here are the steps you need to take:
1. In VS Code, close the tabs from the previous exercise --> Open *lesson3_1.html* by dragging
1. Identify buttons to configure (*Ctrl+F for 'onclick'*)
1. Notice there are two things you need to add:
    1. When clicked, the change_viz function is empty right now. You need to add the correct Tableau Server viz URL to the function.
    1. If you scroll to the right, the View Name within the span element needs to be updated to the correct viz name.
1. Use the following vizzes for the three buttons:
    1. workbook_@lab.CloudCredential(TC2023-HOT20).Username --> My TC Dash dashboard (*Call it 'TC Dashboard'*)
    1. workbook_@lab.CloudCredential(TC2023-HOT20).Username --> Sales Map sheet (*Call it 'Sales Map'*)
    1. Samples project --> Regional workbook --> Global Temperatures dashboard (*Call it 'Temperatures'*)
1. Save your changes
1. Switch to *embed3_1.js* file
1. Declare the viz object and set it to the document element with id '<b>tableauViz</b>' (*look at embed1.js for example*)
1. Configure the change_viz(url) function to set the viz source property to url of the new viz (*the url passed into the function*)
1. Save your changes and reload TC23 app in non-incognito browser --> Test out buttons to switch vizzes

If you did things correctly, you may notice that the first two buttons work but the last one (for the Temperatures dashboard) does not. If this happens to you, perhaps you need to check the scopes on your CA in Tableau Cloud (hint: *this is the same issue encountered in exercise 2 and was resolved by changing the CA project scope*)

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

<br/>

---
>[!note]###Complete exercise 3.2.1
For this exercise, you are going to:
- Review the html for a static categorical filter
- Call the right js methods to apply filter behavior

To get started:
- Switch back to the TC23 app in your non-icognito browser window --> Logout
- Click --> *Lesson 3: Advanced API v3*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click *Lesson 3_2_1: Static Filters* link
- Expand the *Filter* button on the left hand side. This reveals five individual buttons that should filter (or clear) the embedded dashboard. In this exercise, you will configure these buttons to work.

Here are the steps you need to take:
1. In VS Code, close the tabs from the previous exercise --> Open *lesson3_2_1.html*
1. Identify buttons to configure (*Ctrl+F for 'filterTableau'*)
1. Notice the two custom functions *filterTableau* and *filterClear*. Pay attention to what is passed into both functions. In *filterTableau*'s case, it's the name of the field and the categorical values to filter out. In *filterClear*'s case, it's the name of the field to clear.
1. Open *embed3_2_1.js*
1. Scroll down to the for-loop to find code that loops through a dashboard's individual sheets to find the sheet to filter on.
1. Add name of the sheet we want to filter on (*Sales Map*) to the case statement
1. Scroll down to the *filterTableau* function and add code to call the *applyFilterAsync* method on the *activeFilterSheet* object [as shown here](https://help.tableau.com/current/api/embedding_api/en-us/reference/interfaces/worksheet.html#applyfilterasync).
1. Scroll down to the *filterClear* function and add code to call the *clearFilterAsync* method on the *activeFilterSheet* object [as shown here](https://help.tableau.com/current/api/embedding_api/en-us/reference/interfaces/worksheet.html#clearfilterasync).
1. Save your changes and reload TC23 app in non-incognito browser --> Test out filter buttons

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

<br/>

---
>[!note]###[OPTIONAL] Complete exercise 3.2.2
For this exercise, you are going to:
- Review the html for a dynamic categorical filter
- Call the right js methods to apply filter behavior

To get started:
- Switch back to the TC23 app in your non-icognito browser window --> Logout
- Click --> *Lesson 3: Advanced API v3*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click *Lesson 3_2_2: Dynamic Filters* link
- Expand the *Dynamic Filter* button on the left hand side. Right now, the button is empty. In this exercise, you will configure it to work.

Here are the steps you need to take:
1. Switch to Tableau Cloud in your incognito browser window, open the Sales Map sheet and click the Edit button at the top. Once in edit mode, look in the top left corner to identify the other categorical filter applied to this sheet besides Region. **Make note of the name of this filter.** We will use it in a later step --> Hit the X to close out web edit mode
1. In VS Code, close the tabs from the previous exercise --> Open *lesson3_2_2.html*
1. Identify button to configure (*Ctrl+F for 'dynFilter'*)
1. Notice that we have carved out space where we will dynamically populate the html with filter values.. nothing to add here
1. Switch to *embed3_2_2.js*
1. Scroll down to the for-loop to find code that loops through a dashboard's individual sheets to find the Sales Map sheet and get all the filters applied to this sheet.
1. Scroll down to the *getFilters* function and add the name of the remaining filter to the blank if-statement.
1. Review *domain* function to see how we can loop through the array of filter values and input into our html file.
1. Save your changes and reload TC23 app in non-incognito browser --> Confirm dynamic filter list is populated and buttons work

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

<br/>

---
>[!note]###[Optional] Complete exercise 3.3
For this exercise, you are going to:
- Configure 3 html buttons to select specific states
- Call the right js method to apply selection

To get started:
- Switch back to the TC23 app in your non-icognito browser window --> Logout
- Click --> *Lesson 3: Advanced API v3*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click *Lesson 3_3: Select Marks* link
- Expand the *Select Marks* button on the left hand side. It expands to 4 individual buttons. In this exercise, you will configure them to work.

Here are the steps you need to take:
1. In VS Code, close the tabs from the previous exercise --> Open *lesson3_3.html*
1. Identify buttons to configure (*Ctrl+F for 'selectTableau'*)
1. For the East, Central, and West buttons, add a couple states to each array
1. Save your changes
1. Switch to *embed3_3.js*
1. Identify sheet we will be selecting marks on (*Ctrl+F for '3_3'*)
1. Find SelectTableau function (*Ctrl+F for 3_3*)
1. Call on relevant method to select marks. Find a [useful example here](https://help.tableau.com/current/api/embedding_api/en-us/reference/interfaces/worksheet.html#selectmarksbyvalueasync). Pay close attention to the syntax.
1. Also note the *selectClear()* function and its relevant method.
1. Save your changes and reload TC23 app in non-incognito browser --> Test buttons to select different states by region

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

<br/>

---
>[!note]###Complete exercise 3.4
For this exercise, you are going to:
- Wire up an event listener for mark selections
- Grab mark selection info to dynamically open a wikipedia page

To get started:
- Switch back to the TC23 app in your non-icognito browser window --> Logout
- Click --> *Lesson 3: Advanced API v3*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click *Get Marks* link
- At the moment, nothing new happens when you select an individual state on the map

Here are the steps you need to take:
1. In VS Code, close the tabs from the previous exercise --> Open *lesson3_4.html*
1. Identify empty div element to populate with selected mark (*Ctrl+F for 'Lesson 3_4'*)
1. Switch to *embed3_4.js*
1. Under Lesson 3_4 comments, add an event listener which listens for mark selections. Look at the existing event listener located right above it for a working example. (hint: instead of "firstinteractive", this event listener fires on "markselectionchanged")
1. Scroll to the *getSelectedMarks(marksEvent)* function and follow the flow of the code
1. Save your changes and reload TC23 app in non-incognito browser --> Click on a State and see what pops up underneath the dashboard

>[!note]If you're completely stuck, you can see the [Solution Code Snippets](#solution-code-snippets)

===

#Lesson 4: User Attribute Functions

>Here are the high level steps you need to take:
1. [] Load Dashboard without filtering
1. [] Configure User Attribute Function in Workbook
1. [] Pass User Attributes into JWT
1. [] Test Dashboard with filtering

<br/>

In the previous section, you tested some of the more advanced features of the Embedding API v3. Let's now pivot to using User Attribute Functions to secure the data by passing data authorizations into the JWT at runtime. This will ensure that users see the right rows of data in a multitenant data architecture.

---

###Step 1: Load Dashboard without filtering
- Switch back to the TC23 app in your non-incognito browser window --> Logout
- Click --> *Lesson 4: User Attribute Functions*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click Load Your Dashboard with UAF! --> Notice the dashboard currently loads all of the data (no filtering)

<br/>
###Step 2: Configure User Attribute Function in Workbook

The first step is to add the UAF to the workbook itself, and specifically to the embedded data source. Think of this as the plumbing we need to do for the feature to work. It is the mechanism used to apply the filter values specified in the JWT.

- Switch to Tableau Cloud Site in your incognito browser window
- Open the side menu --> Click Explore
- Click the filter toggle on the right
- Search for the *@lab.CloudCredential(TC2023-HOT20).Username* Project --> Open your project (**Make sure it's your project not someone elses**)
- Click *workbook_@lab.CloudCredential(TC2023-HOT20).Username*
- Click *My TC Dash*
- Click Edit (at the top) --> Switch to *Sales Map*
- Click on Analysis (top menu) --> Create Calculated Field --> Name it "UAF Calc"

Enter the following: +++USERATTRIBUTEINCLUDES('myuaf', [State])+++

- Click OK
- Click on Data (top left) --> Superstore Data --> Edit Data Source Filters
- Click Add Filter --> UAF Calc --> Exclude selected values --> Select False checkbox --> OK --> OK

You have just secured the User Attribute logic by adding it as a filter to the embedded data source. **Click the blue Publish button to apply your changes to the workbook.** (If you see *Publish As* instead of *Publish*, you're on the wrong workbook. Please switch to correct workbook.)

Click Go to Workbook link (at the top) after publishing. Notice now there are user filters applied to the thumbnails as well.

<br/>
###Step 3: Pass User Attributes into JWT

Ok the plumbing for row level security is done. We now just have to pass the user attribute values directly into a JWT claim correctly.

- Switch to VS Code, close the tabs from the previous exercise --> Open *EmbedPortal.py* (We don't need to make any edits to our html/js)
- Find the getJWT() function (*Ctrl+F for 'getJWT'*)
- Uncomment the "myuaf" claim
- On the right-hand side of the colon, enter an array of states, for ex: +++["Texas", "New York", "Utah"]+++
- Make sure there is still a comma at the end of the array (because there is still another claim that follows)
- Save *EmbedPortal.py*

<br/>
###Step 4: Test Dashboard with filtering
- Switch back to the TC23 app in your non-incognito browser window --> Logout
- Click --> *Lesson 4: User Attribute Functions*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Click Load Your Dashboard with UAF! --> Notice the dashboard now filters the data to only the states you specified in the JWT claim

Important: If you wish to try a different set of values, make the changes in the JWT, and then **Logout of the TC23 app and log back in to Lesson 4 again**.

Well done! You delivered SSO authentication & row level security together through the same mechanism (Connected Apps).

===

#Lesson 5: Complete Solution
CONGRATULATIONS! You've made it until the end. You've officially graduated from embedding bootcamp!

!IMAGE[class-of-23.jpg](class-of-23.jpg)

Now you get to try the full solution with all the integrations you worked on.

<br/>

>To test:
- Switch back to the TC23 app in your non-incognito browser window --> Logout
- Click --> *Lesson 5: Complete Solution*
- Login with external Username: +++@lab.CloudCredential(TC2023-HOT20).Usernameext@mytc23.com+++
- Select a state and make sure the wikipedia page loads
- Run through all the buttons on the left and verify they work
- Have a look at *complete.html* and *complete.js* for the entire working code

<br/>

Which integration do you like the most? Which one do you find the most useful?

Thank you so much for joining us today and choosing to spend time learning about embedded analytics with Tableau.

We would appreciate it if you could provide honest feedback in the Salesforce Events mobile app! That's how we know what we're doing well and what we can improve on for next year. Thank you!

===

#Solution Code Snippets

##Lesson code snippets

Please use this section to show snippets of the completed code. If you wish to copy them into your working code, it's best to launch notepad, and copy into that first, before copying into vscode. This prevents the automatic copy/paste from inserting code in the wrong place! 

@lab.DropDownList(bob)[Select,Lesson1,Lesson2,Lesson3_1,Lesson3_2_1,Lesson3_2_2,Lesson3_3,Lesson3_4]

:::Code(bob=Select)
Select a Lesson in the drop down to show the code snippets relevant for your exercise
:::

:::Code(bob=Lesson1)
```HTML
<tableau-viz id="tableauViz"
    src= "{{tabServer}}/t/{{tabSite}}/views/{{tabWorkbook}}/MyTCDash"
        device="mobile" toolbar="hidden" hide-tabs width=100%>
</tableau-viz>
```
```JavaScript
// revertAll function
viz.revertAllAsync();
```
```JavaScript
// set viz object
viz = document.getElementById('tableauViz');

// undo function
viz.undoAsync();
```
```JavaScript
// set viz object
viz = document.getElementById('tableauViz');

// redo function
viz.redoAsync();
```
:::

:::Code(bob=Lesson2)

>[!alert] Do not simply copy and paste the python code, it's just an example of what it should look like. Your values will differ.

```Python-nocopy
CA_SSO_token = jwt.encode(
    {
        # Lesson 2
        # https://help.tableau.com/current/online/en-us/connected_apps.htm#step-3-configure-the-jwt
        # Set iss (Issuer) to Connected App Client ID
        "iss": '4063aef9-f1ef-4dac-87bc-06a05f21a64b',
        "exp": datetime.datetime.utcnow() + datetime.timedelta(minutes=10),
        "jti": str(uuid.uuid4()),
        "aud": "tableau",
        "sub": username,
        "scp": ["tableau:views:embed", "tableau:metrics:embed"]
    },
    # Set this value to the Connected App Secret Key 
    'PBBgI0D9Pe0inqNf+nHy/K6Ytv8mP06bjkcoOtfx6Lk=',
    algorithm="HS256",
    headers={
        # Set kid (Key ID) to Connected App Secret ID
        'kid': 'df4b89a3-7761-4053-9e4f-77958e00e18f',
        # Set iss (Issuer) to Connected App Client ID
        'iss': '4063aef9-f1ef-4dac-87bc-06a05f21a64b'
    }
)
```

```Connected_App_config
Look at your CA Scopes in Tableau Cloud.

Did you properly scope the CA to the right project(s)?
```
:::

:::Code(bob=Lesson3_1)

```HTML
<a class="collapse-item" href="#" onclick="change_viz('https://eu-west-1a.online.tableau.com/t/embeddedhot/views/workbook_@lab.CloudCredential(TC2023-HOT20).Username/MyTCDash');"><i class="fas fa-fw fa-image"></i><span>TC Dashboard</span></a>
<a class="collapse-item" href="#" onclick="change_viz('https://eu-west-1a.online.tableau.com/t/embeddedhot/views/workbook_@lab.CloudCredential(TC2023-HOT20).Username/SalesMap');"><i class="fas fa-fw fa-image"></i><span>Sales Map</span></a>
<a class="collapse-item" href="#" onclick="change_viz('https://eu-west-1a.online.tableau.com/t/embeddedhot/views/Regional/GlobalTemperatures');"><i class="fas fa-fw fa-image"></i><span>Temperatures</span></a>
```

```JavaScript
// window.onload function
viz = document.getElementById('tableauViz');

// chnage_viz function
viz.src = url;
```
:::

:::Code(bob=Lesson3_2_1)

```JavaScript
// case statement
case 'Sales Map':
```
```JavaScript
// filterTableau function
activeFilterSheet.applyFilterAsync(filterName, value, action);

// filterClear function
activeFilterSheet.clearFilterAsync(filterName);
```
:::

:::Code(bob=Lesson3_2_2)
```JavaScript
// if statement
if (filter.fieldName == "Category")
```

:::

:::Code(bob=Lesson3_3)

```HTML
<!-- Example, but you can use any States you like -->
<a class="collapse-item" href="#" onclick="selectTableau('State',['Virginia','Vermont'])">East</a>
<a class="collapse-item" href="#" onclick="selectTableau('State',['Wisconsin','Texas'])">Central</a>
<a class="collapse-item" href="#" onclick="selectTableau('State',['Washington','Utah'])">West</a>
```

```JavaScript
// function selectTableau()
  activeSelectSheet.selectMarksByValueAsync([{
     fieldName: fieldName,
     value: value
  }], action );
```

:::

:::Code(bob=Lesson3_4)
```JavaScript
// add the event listener
viz.addEventListener("markselectionchanged", getSelectedMarks);
```
:::

<br/>

---
#### Click on a link below to navigate back to a section:
[Initial Setup](#initial-setup)

[Lesson 1: Simple Embedding](#lesson-1-simple-embedding)

[Lesson 2: Connected Apps](#lesson-2-connected-apps)

[Lesson 3: Advanced APIv3](#lesson-3-advanced-apiv3)

[Lesson 4: User Attribute Functions](#lesson-4-user-attribute-functions)

[Lesson 5: Complete Solution](#lesson-5-complete-solution)
