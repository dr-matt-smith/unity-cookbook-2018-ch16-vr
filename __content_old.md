# Virtual Reality and extra features

In this chapter, we will cover:

1. Pausing the game
2. Implementing slow motion
3. Gizmos
4. Optimization with the Unity Profiler
5. Particles
6. Recording in Unity 
   
1. Creating a simple VR scene
1. Deploying a simple VR scene to a standalone executable
1. Using a Windows batch file to force VR mode when running an executable
1. Balancing Sharpness and Performance by changing the Render Scale 
1. User Interaction with the VREyeCaster
1. Gesture input with VRInput events
1. Object interaction with the VRInteractiveItem component
1. Gaze selection with a visible reticle
1. Displaying a clock to explore Spatial UI 
1. Free anti-aliasing on text with a Canvas Scaler on a Worldspace Canvas
1. Fading when changing positions (to avoid VR Sickness nausea)
1. Optimizing for VR applications


<!-- ******************************* -->
<!-- ******************************* -->

# Introduction

There are too many features in Unity 2018 to all be covered in an single book. In this chapter we present a set of 
recipes illustrating VR game development in Unity, plus a range of additional Unity features.

# The Big Picture

Virtual Reality is about presenting to the player an immersive audio-visual experience, engaging enough for them to 
lose them selves in exploring and interacting with the game world that has been created.

From one point of view, VR simply requires 2 cameras, to generate the images for each eye, to give that realistic 3D 
effect. 


xxxx

xxxx


xxx


<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Pausing the game

Games are more fun when there is a leaderboard of high scores that the players have achieved. Even single player games can communicate to a shared web-based leaderboard. This recipe creates the web-server side (PHP) scripts to set and get the player scores from an SQL database. The next recipe then creates a Unity client that can communicate with this web server.


<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

This recipe assumes that you either have your own web hosting, or are running a local web server. You could use the built-in PHP webserver, or a web server like Apache or Nginx. For the database you could use an SQL database server such as MySQL or MariaDB, however, we've
 tried to keep things simple using SQLite - a file-based databse system. So in fact, all you actually need on your 
 computer is PHP 7, since it has a built-in webserver and can talk to SQLite databases, which is the setup on which 
 this recipe was tested.
 

All the PHP scripts for this recipe, and the SQLite databse file, can be found in the 12_01 folder.


<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To set up a leaderboard using PHP and a database, do the following:

1. Copy the provided PHP project to where you will be running your webserver:

    - live website hosting: copy the files to the live web folder on your server (often www or httdocs)
    
    - running on local machine: at the comment line you can use the Composer script shortcut to run the PHP built-in 
    websever by tying: composer run
    
    ![Insert Image B08775_12_03.png](./12_figures/B08775_12_03.png)

    
1. Open a web browser to your website location:

    - live website hosting: visit the URL for your hosted domain
    
    - running on local machine: visit URL: localhost:8000

    ![Insert Image B08775_12_04.png](./12_figures/B08775_12_04.png)

1. First create/reset the database, by clicking the last bulleted link: reset database. You should see a page with 
message database has been reset, and a link back to the home page (click that link).

1. Next, view the leaderboard scores as a web page in your web browser, click the second link: list players (HTML)

    ![Insert Image B08775_12_05.png](./12_figures/B08775_12_05.png)

1. Now try the 5th link - to retreive the leaderboard data as a text file. Note how it looks different viewed in the 
web browser (which ignores line breaks), that when you view the actual source file returned from the server.

    ![Insert Image B08775_12_06.png](./12_figures/B08775_12_06.png)

1. Do the same with the JSON and XML options - to see how our server can return the contents of the database wrapped 
up as HTML, plain text (TXT), XML or JSON.

1. Now click the 6th link: create (username = mattilda, score=800). When you next retrieve the contents you'll see 
that there is a new database record for player mattilda with score 800. This shows that our server can receive data 
and change the contents of the database, as a well as just returning values from it.


<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

The player's scores are stored in an SQLLite database. Access to the database is facilitated through the PHP scripts provided. In our example, all the PHP scripts were placed in a folder on our local machine, from which we'll run the server (it can be anywhere when using
 the PHP built-in server). So, the scripts are accessed via  http://localhost:8000. 

All the access is through the PHP file called index.php. This is called a Front Controller, acting like a receptionist in a building, interpreting requests and asking the appropriate function to execute some actions and return a result in response to the request.

There are five actions implemented, and each is indicated by adding the action name at the end of the URL (this is the GET HTTP method, which is sometimes used for web forms. Take a look at the address bar of your browser next time you search Google for example). The actions and their parameters (if any) are as follows:


- action = list & format = HTML / TXT / XML / JSON 

    - This action asks for a listing all player scores to be returned. Depending on the  value of second variable 
    format (html/txt/xml/json), the list of users and their scores is return in different text file formats

- action = reset

    - This action asks for a set of default player name and score values to replace the current contents of the database table. This action takes no argument. It returns: some HTML stating that the database has been reset, and a link to the home page
    
- action = get & username = <usermame> & format = HTML / TXT

    - This action asks for the integer score of the named player that  is to be found. It returns: the  score integer
    . There are 2 formats, HTML for a webpage giving the player's score, and TXT where the numeric value is the 
    only content in the HTTP message returned
    
- action = update & username = `<usermame>` & score = `<score>`

    - This action asks for the provided score of the named player to be stored in the database (but only if this new score is greater than the currently stored score). It returns: the word success (if the database update was successful), otherwise -1  (to indicate that
     no update took place).


<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some ways to go further with this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## SQLite, PHP and database servers

The PHP code in this recipe used the PDO data objects functions for communicating with an SQLite local file-based database. Learn more about PHP and SQLite at:

- http://www.sqlitetutorial.net/sqlite-php/

When SQLite isn't a solution (e.g. not suported by a web hosting package), you may need to develop locally with an 
SQL server such as MySQL Community Edition or MariaDB, and then deploy with a live database server from your hosting 
company.

A good solution to try things out on your local machine can be a combined web application collection, such as XAMP/WAMP/MAMP. Your web server needs to support PHP, and you also need to be able to create the MySQL databases.


<!-- ******************************** -->
<!-- ******************************** -->

## PHPLiteAdmin

When writing code that talks to database files and database servers, it can be frustrating when things are not 
working to not be able to see **inside** the database. Therefore database clients exist to allow you to interact with
 database servers withing having to use code.
 
A lightweight (single file!) solution when using PHP and SQLLite is PHPLiteAdmin, which is free to use (although you 
may consider donating on their website link below if you use it a lot). It is included in the folder phpLiteAdmin 
folder with this recipe's PHP scripts. It can be run by using the Composer script shortcut command: composer dbadmin,
 and will run locally at: localhost:8001. Once runing just click on the lnk for the players table, to see the data 
 for each player's score in the database file:
 
![Insert Image B08775_12_07.png](./12_figures/B08775_12_07.png)
 
Learn more about PHPLiteAdmin at the projects GitHub respository and website:

- https://github.com/phpLiteAdmin/pla

- https://www.phpliteadmin.org/

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Unity game communication with web-sever leaderboard

In this recipe we create a Unity game client that can communicate, via UI buttons etc., with our web-server leaderboard from the previous recipe.

![Insert Image B08775_12_08.png](./12_figures/B08775_12_08.png)

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

Since the scene contains several UI elements and the code of the recipe is the communication with the PHP scripts and SQL database, in 12_02 folder, we have provided a Unity package called UnityLeaderboardClient, containing a scene with everything set up for the Unity project.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To create a Unity game communication with web-sever leaderboard, do the following:

1. Import the provided Unity package called UnityLeaderboardClient.

1. Run the provided Scene.

1. Ensure your PHP leaderboard is up and running.

1. If you are not running locally (localhost:8000), then you'll need to update the URL, by selecting the Main Camera in the Hierarchy, and then editing the Leader Board URL text for the Web Leaderboard (Script) component in the Inspector.

    ![Insert Image B08775_12_09.png](./12_figures/B08775_12_09.png)

1. Click on the buttons to make Unity communicate with the PHP scripts that have access to the high score database.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

The player's scores are stored in an SQL database. Access to the database is facilitated through the PHP scripts provided - by the web server project setup in the previous recipe. 

In our example, all the PHP scripts were placed in a  web server  folder for a local webserver. So, the scripts are accessed via  http://localhost:8000/. However, since URL is a public string variable, this can  be set before running to the location of your server and site code.

There are buttons in the Unity scene (corresponding to the actions the web leaderboard understands) which set up the corresponding action and the parameters to be added to the URL, for the next call to the web server, via the LoadWWW() method. The OnClick actions have been set up for each button  to call the corresponding methods of the WebLeaderBoard C# script of the Main Camera.

There are also several UI Text objects. One displays the most recent URL string sent to the server. Another displays the integer value that was extracted from the response message that was received from the server (or a message as "not an integer" if some other data was received). The third UI Text object is inside a panel, and has been made large enough to display a full, multi-line, text string, received from the server (which is stored inside the textFileContents variable).

We can see that the contents of the HTTP text reponse message is simply an integer when a random score is set for 
player Matt, when the Get score for player 'matt' (TXT) button is clicked, and a text file containing 505 is returned:

![Insert Image B08775_12_10.png](./12_figures/B08775_12_10.png)


The UI Text objects have been assigned to public variables of the WebLeaderBoard C# script for the Main Camera. When any of the buttons are clicked, the corresponding method of the WebLeaderBoard method is called, which builds the URL string with parameters, and then calls the LoadWWW() method. This method sends the request to the URL, and waits (by virtue of being a coroutine) until a response is received. It then stores the content, received in the textFileContents variable, and calls the UpdateUI() method. There is a 'prettification' of the text received, inserting newline characters to make 
the JSON, HTML and XML easier to read.

<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some ways to go further with this recipe.
<!-- ******************************** -->
<!-- ******************************** -->

## Extracting the full leaderboard data for display within Unity

The XML/JSON text that can be retrieved from the PHP web server provides a useful method for allowing a Unity game to retrieve the full set of the leaderboard data from the database. Then, the leaderboard can be displayed to the user in the Unity game (perhaps, in some nice 3D fashion, or through a game-consistent UI). 

See other chapters in this cookbook to learn how to process data in XML and JSON formats.

<!-- ******************************** -->
<!-- ******************************** -->

## Using the secret game codes to secure your leaderboard scripts

The Unity and PHP code that is presented illustrates a simple, unsecured web-based leaderboard. To prevent players hacking into the board with false scores, it is usual to encode some form of secret game code (or key) into the communications. Only update requests that include the correct code will actually cause a change to the database.

The Unity code will combine the secret key (in this example, the string called harrypotter) with something related to the communication—for example, the same MySQL/PHP leader board may have different database records for different games that are identified with a game ID:

```csharp
    // Unity Csharp code
    string key = "harrypotter"
    string gameId = 21;
    string gameCode = Utility.Md5Sum(key + gameId);
```

The server-side PHP code will receive both the encrypted game code, and also the piece  of game data that is used to create that encrypted code (in this example, the game ID and MD5 hashing function, which is available in both, Unity and in PHP). The secret key (harrypotter) is used with the game ID to create an encrypted code that can be compared with the code received from the Unity game (or whatever user agent or browser is attempting to communicate with the leaderboard server scripts). The database actions will only be executed if the game code created on the server matches that send along with the request  for a database action.

```php
    // PHP – security code
    $key = "harrypotter"
    $game_id =  $_GET['game_id'];
    $provided_game_code =  $_GET['game_code'];
    $server_game_code = md5($key.$game_id);
    
    if( $server_game_code == $provided_game_code ) {
      // codes match - do processing here
    }
```


<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Creating and cloning a Githuub repository

Distributed Version Control Systems (DVCS) are becoming a bread-and-butter everyday tool for software developers. An issue with Unity projects can be the many binary files in each project. There are also many files in a local system's Unity project directory that are not needed for archiving/sharing, such as OS specific thumbnail files, trash files, and so on. Finally, some Unity project folders themselves do not need to be archived, such as Temp  and Library.

While Unity provides its own Unity Teams online collaboration, many small game developers chose not to pay for this extra feature. Also, Git and Mercurial (the most common DVCSs) are free, and work with any set of documents that are to be maintained (programs in any programming language, text-files, and so on). So, it makes sense to learn how to work with a third-party, industry standard DVCS for the Unity projects. In fact, the documents for this very book were all archived and version-controlled using a private GitHub repository!

In this project, we'll create a new online project repository using the free GitHub server, and then clone (duplicate) a copy onto a local computer. The recipe that follows will then transfer a Unity project into the local project repository, and use the stored link from the cloning to push the changed files back up to the Guthub online server.

NOTE: **All** the projects from this Cookbook have been archived on GitHub in public repositories for you to read, 
download, edit and run on your computer. Depsite a hard disk crash during the authoring of this book no code was 
lost, since the steps of this recipe were being followed for every part of this book.

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

Since this recipe illustrates hosting code on GitHub, you'll need to create a (free) GitHub account at GitHub.com if you do not already have one.

If not already installed, you'll need to install Git on your local computer as part of this recipe. Learn how, and download the client from the following links:

- http://git-scm.com/book/en/Getting-Started-Installing-Git

- http://git-scm.com/downloads/guis

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To create and clone a GitHub repository, do the following:

1. Install Git for the command line on your computer. As usual, it is good practice to do a system backup **before** installing any new application.

    - https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

1. Test that you have Git installed, by typing git at the command line in a terminal window:

    ![Insert Image B08775_12_02.png](./12_figures/B08775_12_02.png)

1. Open a web browser and nagivate to your Gihhub repositories page:

    ![Insert Image B08775_12_11.png](./12_figures/B08775_12_11.png)

1. Click the green button to start creating a new repository (e.g. my-unity-demo). Choose:
 
    - enter a name for the new repository, e.g. my-unity-demo 
    
    - click the option to create a README (important, so you can **clone** the files to a local computer)
    
    - add a .gitignore file - choose the Unity one
       
    ![Insert Image B08775_12_01.png](./12_figures/B08775_12_01.png)

    NOTE: The .gitignore file is a special file, that tells the version control system which files do not need to be archived. For example, we don't need to record the Windows or Mac image thumbnail files (DS_STORE or Thumbs.db).

1. With the options selected, click the green Create Repository button.

1. You should now be taken to the repository contents page. Click the green dropdown named Clone or Download, and then click the URL copy-to-clipboard tool button. This has copied the special GitHub URL needed for connecting to GitHub and copying the files to your local computer.

    ![Insert Image B08775_12_12.png](./12_figures/B08775_12_12.png)

1. Open a Terminal window, and navigate to the location to which you wish to clone your GitHub project repository (e .g. Desktop or Unity-projects).

1. At the CLI (Command Line Interface) type: git clone, and then paste the URL from your clipboard, it will be something like https://github.com/dr-matt-smith/my-unity-github-demo.git.

    ![Insert Image B08775_12_13.png](./12_figures/B08775_12_13.png)

1. Change into your cloned directory, e.g.: cd my-unity-github-demo.

1. List the files. You should see your README.md, and if you have the option to see hidden folders and files you'll 
also see .git and .gitignore.

    ![Insert Image B08775_12_14.png](./12_figures/B08775_12_14.png)

1. Finally, use the command git remote -v to see the stored link from this copy of the project files on your 
computer, back up to the GitHub online repostory:

    ```
        $ git remote -v
        origin  https://github.com/dr-matt-smith/my-unity-github-demo.git (fetch)
        origin  https://github.com/dr-matt-smith/my-unity-github-demo.git (push)
    ```

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

You learned how to create a new empty repository on the GitHub web server. You then **cloned** it to your local computer. 

You also check to see that this clone had a link back to its remote origin.

NOTE: If you have simply downloaded and decompressred the ZIP file, you would not have the .git folder, not have the 
remote links back to its GitHub origin.

The special file called .gitignore lists all the files and directories that are not  to be archived. At the time of writing here are the contents of files that do **not** need to be archtives (they are either unnecessary or can be re-generated when a project is loaded into Unity):

```
    [Ll]ibrary/
    [Tt]emp/
    [Oo]bj/
    [Bb]uild/
    [Bb]uilds/
    Assets/AssetStoreTools*
    
    # Visual Studio cache directory
    .vs/
    
    # Autogenerated VS/MD/Consulo solution and project files
    ExportedObj/
    .consulo/
    *.csproj
    *.unityproj
    *.sln
    *.suo
    *.tmp
    *.user
    *.userprefs
    *.pidb
    *.booproj
    *.svd
    *.pdb
    *.opendb
    
    # Unity3D generated meta files
    *.pidb.meta
    *.pdb.meta
    
    # Unity3D Generated File On Crash Reports
    sysinfo.txt
    
    # Builds
    *.apk
    *.unitypackage 
```

As we can see, folders like the Library and Temp are not to be archived. Note that if you have a project with a lot of resources (such as those using the 2D or 3D Gamekits), rebuilding the Library may take some minutes depending on the speed of your computer.

Note that the recommended files to ignore for Git change from time to time as Unity change their project folder structure. GitHub has a master recommended .gitignore for Unity, and it is recommended you review it from time to time, especially when upgrading to a new version of the Unity editor:

- https://github.com/github/gitignore/blob/master/Unity.gitignore

NOTE: If you are using an older (pre-2018) version of Unity, you may need to look up what the appropriate .gitignore contents should be. The details given in this recipe were up to date for the 2018.2 version of the Unity editor.

<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some ways to go further with this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Learn more about Distributed Version Control Systems (DVCS) in general

The following video link is a short introduction to DVCS:

- http://youtu.be/1BbK9o5fQD4

Note that the Fogcreek Kiln "harmony" feature now allows seamless work between GIT and Mercurial with the same Kiln repository:

- http://blog.fogcreek.com/kiln-harmony-internals-the-basics/


<!-- ******************************** -->
<!-- ******************************** -->

## Learn more about Git at the command line

If you are new to working with a CLI (Command Line Interface), it is well worth following up some online resources to
 improve your skills. Any serious software development will probably involve some work at a command line at some point.

Since both Git and Mercurial are open source, there are lots of great, free online resources available. The following are some good sources to get started on:

- Learn all about Git, download free GUI clients, and even get free online access to  The Pro Git book (by Scott Chacon), available through Creative Commons license at the following URL:

    - http://git-scm.com/book

- You will find an online interactive Git command line to practice in:

    - https://try.github.io/levels/1/challenges/1

<!-- ******************************** -->
<!-- ******************************** -->

## Using Bitbucket and SourceTree visual application

Unity offer a good tutorial on version control using the Bitbucket web site and the SourceTree application:

- https://unity3d.com/learn/tutorials/topics/cloud-build/creating-your-first-source-control-repository

SourceTree is a free Mercurial and Git GUI client, available at:

- http://www.sourcetreeapp.com/

<!-- ******************************** -->
<!-- ******************************** -->

## Learning about Mercurial rather than Git

The main Mercurial website, including free online access to the Mercurial:  The Definitive Guide (by Bryan O'Sullivan) book is available through the Open Publication License at:

- http://mercurial.selenic.com/



<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Adding a Unity project to a Git repository, and pushing it up to GitHub

In the previous recipe you created a new online project repository using the free GitHub server, and then cloned (duplicate) a copy onto a local computer. 

In this recipe we will transfer a Unity project into the local project repository, and use the stored link from the cloning to push the changed files back up to the Guthub online server.

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

This recipe follows on from the previous one, so ensure you've completed that receipe before beginning.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To add a Unity project to a Git repository, and push it up to GitHub, do the following:

1. Create a new Unity project (or make use of an old one), and save the Scene and quit Unity. For example, we created a project named project-for-version-control containing the default SampleScene and a Material named m_red. It is the asset files in the Project panel that are the files that are stored on disk, and are the ones you'll be version controlling with Git and GitHub. 

    NOTE: It is important that all work has been saved and the Unity application is not running when you are archiving your Unity project, since if Unity is open there may be unsaved changes that will not get correctly recorded.

1. On your computer copy the following folders into the folder of your cloned GitHub repository:

    ```
        /Assets
        /Plugins (if this folder exists - it may not)
        /ProjectSettings    
        /Packages
    ```
    
    ![Insert Image B08775_12_15.png](./12_figures/B08775_12_15.png)

1. At the CLI (Command Line Interface) type: git status, to see a list of folders/files that have changed and need to
 be committed to the next snapshot of project contents for out Git version control system.
 
1. Add all these files by typing: git add .

1. Commit our new snapshot with command: git commit -m "files added to project"

    ```
        matt$ git commit -m "files added to project"
        
        [master 1f415a3] files added to project
         23 files changed, 1932 insertions(+)
         create mode 100644 Assets/Scenes.meta
         create mode 100644 Assets/Scenes/SampleScene.unity
         ...        
    ```

1. Now we have created a snapshop of the new files and folders, we can push this new committed snapshot up to the GitHub cloud servers. Type: git push origin master

    ```
        matt$ git push origin master
        
        Counting objects: 29, done.
        Delta compression using up to 4 threads.
        Compressing objects: 100% (27/27), done.
        Writing objects: 100% (29/29), 15.37 KiB | 0 bytes/s, done.
        Total 29 (delta 0), reused 0 (delta 0)
        To https://github.com/dr-matt-smith/my-unity-github-demo.git
           1b27686..1f415a3  master -> master
        matt$ 
    ```

    NOTE: The first time you do this, you'll be asked for your GitHub username and password.
    
1. Now visit GitHub, you should see that there is a new commit, and that your Unity files and folders have been uploaded to the GitHub online repository. 

    ![Insert Image B08775_12_16.png](./12_figures/B08775_12_16.png)

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

In the previous recipe you had created a project repository on the GitHub cloud server, and then cloned it to your local machine. You then added to cloned project repository folder the files from a (closed) Unity application. These new files were added and committed to a new **snapshot** of the folder, and the updated contents *pushed** up to the GitHub server.

Due to the way Unity works, it creates a new folder when a new project is created. Therefore we had to then turn the contents of this the Unity project folder into a Git repository. There are two ways to do this:

- copy the files from the Unity project to a cloned GitHub repository (so it has remote links setup for pushing back to the GitHub server)
 
- making the Unity project folder into a Git repository, and then either linking it to a remote GitHub repository, or pushing the folder contents to the GitHub server and creating a new online repository at that point in time
 
For people new to Git and GitHub the first procedure, which we followed in this recipe, is probably the simplest to understand. Also it the easiest to fix if something goes wrong - since a new GitHub repo can be created, cloned to the local machine, and the Unity project files copied there, and then pushed up to GitHub (and the old repo deleted), by pretty much following the same set of steps.

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Unity project version control using GitHub for Unity

GitHub have released an Open Source tool integrting Git and GitHub into Unity, which we explore in this recipe.

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

You will need Git installed at the command line on your computer:

- https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

You may need to install Git LFT (Large File Transfer) for the GitHub for Unity package to work correctly:

- https://git-lfs.github.com/


<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To manager Unity project version control using GitHUb for Unity, do the following:

1. Start a new Unity project.

1. Open the Asset Store panel, choose menu: Window | General | Asset Store.

1. In the Asset Store search for GitHub for Unity, download and import this into your project.

    ![Insert Image B08775_12_17.png](./12_figures/B08775_12_17.png)

1. If there is a popup about a newer version, then accept it and download the newer version. This will download as a Unity package (probably in your Downloads folder), which you can then import into your Unity project.

1. Once imported, you should see a Plugins | GitHub folder in your Project panel. You will also now see 2 new items 
on your Window menu, for GitHub and GitHub Command Line:

    ![Insert Image B08775_12_18.png](./12_figures/B08775_12_18.png)

1. Choosing the Window | GitHub Command Line allows you to use the Git commands listed in the previous 2 recipes (it will open at the directory of your Unity project).
 
1. Choosing the Window | GitHub menu item will result in a GitHub panel being displayed. Initially this project is not a Git repository, so it will need to be initializes as a new Git project, which can be performnced by clicing the button Initialize as a git repository for this project.

    ![Insert Image B08775_12_21.png](./12_figures/B08775_12_21.png)

1. You will then be able to see that there is one commit, for the snapshot for this intialization of Git version control tracking for this proect.

    ![Insert Image B08775_12_19.png](./12_figures/B08775_12_19.png)

1. Sign in with your GitHub username and password:

    ![Insert Image B08775_12_20.png](./12_figures/B08775_12_20.png)

1. Open a web browser, log in to your GitHub account, and create a new empty repository (with no extra files - i.e. no README, or gitignore or Licence etc.). 

    ![Insert Image B08775_12_22.png](./12_figures/B08775_12_22.png)

1. Copy the URL for the new repository into your clipboard.

    ![Insert Image B08775_12_23.png](./12_figures/B08775_12_23.png)

1. Back in Unity, for the GitHub panel click the Settings button, and paste the URL for the Remote: Origin property, and click button Save Repository to save this change. Your Unity project is now linked to the remote GitHub cloud repository.

    ![Insert Image B08775_12_24.png](./12_figures/B08775_12_24.png)

1. You may now commit and push changes from your Unity project up to GitHub. 

1. Add some new assets (e.g. a new C# script and a Material named m_red). Ten Click the Changes tab in the GitHub panel, ensure the complete Assets folder is checked (and all its contents), then write a brief summary of the changes, and click the Commit to [master] button.

    ![Insert Image B08775_12_25.png](./12_figures/B08775_12_25.png)

1. You now have a commited snapshot of the new Unity project contents on your computer. Push this new committed 
snapshot up to the GitHub server by clicking the Push (1) button. The (1) indicates that there is one new commited 
snapshot locally that has not yet been pushed - i.e. the local machine is 1 commit ahead of the master on the GitHub 
server.

    ![Insert Image B08775_12_26.png](./12_figures/B08775_12_26.png)


1. Visit the repository on GitHub in your web browser, and you'll see the new committed snapshop of the Unity projet contents have been pushed up from your computer to the GitHub cloud servers.
 
    ![Insert Image B08775_12_27.png](./12_figures/B08775_12_27.png)


<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

The GitHub for Unity package adds a special panel with functionality for the following Git / GitHub actions:

- Initializing a new Git project repository for the current Unity Project

- Signing in with your GitHub username and password credentials

- linking your Unity project's Git history to a remote GitHub online repository

- Committing a snapshot of the changes in the Unity project you wish to record

- Pushing committed changes to the remove GitHub onine repository

<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some ways to go further with this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Further reading about GitHub for Unity

Learn more at the following:

- the project website:

    - https://unity.github.com/

- the Quick Start guide

    - https://github.com/github-for-unity/Unity/blob/master/docs/using/quick-guide.md
    

<!-- ******************************** -->
<!-- ******************************** -->

## Pulling down updates from other developers

The GitHub plugin also provifdes the fearure of being able to pull down changes from the remote GitHub repository to your computer (useful if you are working on multiple computers, or a fellow game developer is helping add features to your game)

NOTE: If you are working with other game developers it is very useful to learn about Git branches. Also, before starting work on a new feature, perform a Pull, to ensure you are then working with the most up to date version of the project.


<!-- ******************************** -->
<!-- ******************************** -->

## Unity Collaborate from Unity Technologies

Although Git and GitHub are used for many Unity projects, they are general purpose version control technologies. 
Unity Technologies offers its own online system for developers and teams to collaboratively work on the same Unity 
projects, however this is a feature that is no longer in the free Unity licence plan.

Learn more about Unity Collaborate and Unity teams on the Unity website at:

- https://unity3d.com/unity/features/collaborate 

- https://unity3d.com/teams



<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Preventing your game from running on unknown servers

After all the hard work you've had to go through to complete your web game project, it wouldn't be fair if it ended up generating traffic and income on someone else's website. In this recipe, we will create a script that prevents the main game menu from showing up unless it's hosted by an authorized server.


<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For this recipe, you will need access to a webspace provider where you can host the game. However, you can test locally using a localhost web server, such as the built-in PHP one, or an AMP Apache or Nginx web server.

You also will need to have installed the Unity WebGL build target.

NOTE: At the time of writing (summer 2018), for MacOS computers there is still an issue adding the WebGL package if it is not included at the time of original installation via Unity Hub. The MacOS WebGL package requires the Unity application to be in folder Applications | Unity. When installed via Unity Hub, it is actually in Applications | Unity | Hub | Editor | 2018.2.2f1. If you have multile verisons, they will each be in the Editor folder. The solution for installing WebGL for a particular Unity version is (a) temporarily move the Aplications | Unity | Hub folder somewhere else (e.g. Desktop), and then to copy (or temporarily move) the contents of your Unity version folder (e.g. for me it was 2018 .2.2f1) into Applications | Unity. Then you can run the WebGL package installer successfully and it will add to the PlaybackEngines folder in Applications | Unity. Finally you can move the Unity applications back where they were for Unity Hub to continue working.
 
 

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it... 

To prevent your web game from being pirated, follow these steps:

1. From the Hierarchy, create a UI Text GameObject named Text-loading-warning, choosing menu: Create | UI | Text. 

1. In the Inspector for the Text component enter text Loading...

1. Set the properties for the Text (Script) component in the Inspector set the Horizontal and Vertical Overflow to Overflow, and use the Rect Transform to align the object in the center of the Scene. Make the font size nice and big (e.g. 50).

1. Create a new C# script-class named BlockAccess, and add an instance-object as a component to GameObejct Text-warning:

    ```php
        using UnityEngine;
        using System.Collections;
        using UnityEngine.UI;
        using UnityEngine.SceneManagement;
        
        public class BlockAccess : MonoBehaviour {
            public Text textUI;
            public string warning;
            public bool fullURL = true;
            public string[] domainList;
            
            private void Start() {
                Text scoreText = GetComponent<Text>();
                if (Application.platform == RuntimePlatform.WebGLPlayer)
                {
                    string url = Application.absoluteURL;
                    if (LegalCopy(url))
                        LoadNextScene();
                    else
                        textUI.text = warning;
                }
            }
            
            private bool LegalCopy(string Url) {
               if (Application.isEditor)
                return true;
            
                for (int i = 0; i < domainList.Length; i++){
                    if (Application.absoluteURL == domainList[i])
                        return true;
            
                    if (Application.absoluteURL.Contains(domainList[i]) && !fullURL)
                        return true;
                }
                
                return false;
            }
            
            private void LoadNextScene() {
                int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
                int nextSceneIndex = currentSceneIndex + 1;
                SceneManager.LoadScene(nextSceneIndex);
            }
       }
    ```

1. From the Inspector, leave the options Full URL checked, and increase Size of Domain List to 1 and fill out Element 0 with the complete URL for your game.  In the Warning field type in sentence: This is not a valid game copy.

    ![Insert Image B08775_12_29.png](./12_figures/B08775_12_29.png)

1. Save your Scene as scene0-loading. Add this Scene to the Build (menu: File | Build Settings...). It should be the first one, with index 0.

1. Create a new scene and change its Main Camera background color to black, and add a UI Text message saying Game would play now.

1. Save this second Scene as scene1-game-playing. Add this Scene to the Build - it should be the second one, with index 1.

1. Now let's build our WebGL files. Again, open the Build Settings panel again, and this time ensure that the deployment platform is WebGL. Then click the Build button, and choose the name and folder to be the location for your built files.

    ![Insert Image B08775_12_30.png](./12_figures/B08775_12_30.png)

1. You should now have a folder containing an HTML file (index.html) and folders Build and TemplateData.

    ![Insert Image B08775_12_31.png](./12_figures/B08775_12_31.png)

1. Now copy these folder contents to the public folder on your web server, and use the browser to access the web page through the web server.

1. If your URL is in the list, you'll see the game play (possible with a short view of the Loading... Scene message). If your URL is not in the list, then you'll see the message This is not a valid game copy, and the game will not process to start playing.

NOTE: If you try to open the **file** using your web browser, you will probably get a WebGL not working error. You must be visiting the HTML page through a web server (e.g. localhost:8000 or from your publicly hosted page)

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

As soon as the scene starts, the script compares the actual URL of the WebGL file to the ones listed in the Block Access component. If they don't match, the next level in the build is not loaded and a message appears on the screen. If they do match, the line of code Application.LoadLevel(Application.loadedLevel + 1) will load the next scene from the build list.



<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some ways to go further with this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Enabling WebGL in Google Chrome browser

For this recipe you'll need WebGL enabled in your web browser. At present our test browser (Google Chrome) has WebGL 
disabled by default. To enabled WebGL in the Google Chrome browser do the following:

1. Open Google Chrome and enter the following URL: chrome://flags

1. Locate the Web GL Draft Extensions (e.g. search for WebGL).

1. Use the dropdown menu to change from Disabled to Enabled

1. Click button RELAUNCH NOW, to restart the application with the new settings active.

    ![Insert Image B08775_12_28.png](./12_figures/B08775_12_28.png)

1. You should now be able to use web pages with embedded WebGL content.

<!-- ******************************** -->
<!-- ******************************** -->

## Improving security by using full URLs in your domain list

Your game will be more secure if you fill out the domain list with complete URLs (such as http://www.myDomain.com/unitygame/game.unity3d). In fact, it's recommended that you leave the Full URL option selected so that your game won't be stolen and published under a URL such as www.stolenGames.com/yourgame.html?www.myDomain.com.

