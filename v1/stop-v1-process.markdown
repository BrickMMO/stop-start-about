<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation provides BrickMMO participants an opportunity to give feedback on the BrickMMO project and process. 

The application frontend will include the following pages:

1. A home page that lists current classes.
2. Once a class is clicked there will be a page to adminster the survey.

The admin will include the following pages:

1. A plage to login.
2. A dashbaord to choose between manage classes and restults.
3. A page to vew the results.
4. A group of pages to add, edit, and delete classes.

*** 

### Completed by Instructor

The [stop-start-v1](https://github.com/BrickMMO/stop-start-v12) includes a basic PHP core also used in [flow-v1](https://github.com/BrickMMO/flow-v2) and [stop-start-continue-v1](https://github.com/BrickMMO/stop-start-continue-v1). And there is a page listing the current classes, a page to administer the survey, and a page to view the results.

There are three tables for this project named `admins`:

```sql
CREATE TABLE `admins` (
  `id` int(11) NOT NULL,
  `first` varchar(255) DEFAULT NULL,
  `last` varchar(255) DEFAULT NULL,
  `github` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  `status` enum('active','inactive') NOT NULL DEFAULT 'active',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `upated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

And `feedback`:

```sql
CREATE TABLE `feedback` (
  `id` int(11) NOT NULL,
  `class_id` int(11) NOT NULL,
  `stop` varchar(255) NOT NULL,
  `start` varchar(255) NOT NULL,
  `continue` varchar(255) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

And `classes`:

```sql
CREATE TABLE `classes` (
  `id` int(11) NOT NULL,
  `name` varchar(255) DEFAULT NULL,
  `code` varchar(255) DEFAULT NULL,
  `semester` enum('fall','winter','summer') DEFAULT NULL,
  `year` year(4) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

*** 

### Steps

1. **Design**

    Using [Figma](https://www.figma.com/) (or your design tool of choice) mock up the application. The application should include three pages:

    1. **Classes**

        A list of current classes in a well presented format. 

    2. **Survey**

        A page with all the stop, start, and continue surbyey form. The surve is three textareas, all on one page.

    3. **Admin**

        Do not worry about the design for the admin for now. 

    The design style should mimic that of the [Passive Aggressive Password Machine](https://trypap.com/):

    - Nice large fonts
    - Simple layout
    - Well spaced out
    - Simple forms

    This project will use [Bootstrap](https://getbootstrap.com/). Review the Bootstrap website and avalable components and incorporate these into your design. 

    When complete, present your design to your instructor before moving on to the next step. 

2. **Local Server**

    Install a tool such as [MAMP](https://www.mamp.info/) on your computer. Open the MAMP progam and start the server. 

    Open the MAMP home page. It should be something like [http://localhost:8888](http://localhost:8888) on a Mac or [http://localhost](http://localhost] on a PC.

    In another tab open up [phpMyAdmin](https://www.phpmyadmin.net/). If you're using MAMP it will be something like [http://localhost:8888/phpMyAdmin/](http://localhost:8888/phpMyAdmin/) on a MAC or [http://localhost/phpMyAdmin/](http://localhost/phpMyAdmin/) on a PC.
    
3. **Database**

    Using phpMyAdmin create a new database named `brickmmo_stopstart` and import the three SQL files in the `/imports` folder. You should now have three empty tables named `admin`, `feedback`, and `admins`.

4. **Source Code**

    Clone the [stopstart-v1](https://github.com/BrickMMO/stopstart-v1) repo into your MAMP root folder (or point MAMP to your cloned directory). Restart the server if needed. 

    Copy the `/.env.sample` file amd name it `/.env`. Change the database variables to `brickmmo_stopstart` and the MAMP defaults:

    ```php
    DB_HOST=localhost
    DB_DATABASE=brickmmo_colours
    DB_USERNAME=root
    DB_PASSWORD=root
    ```

    At this point you can open open the BrickMMO Stop-Start home page. It will display one class.

    > [!TIP]  
    > Steps five through eleven have a project task in GitHub. Make sure to assign these tasks when starting. And then use these tasks to create your branch and a pull request when done.

5. **Home Page**

   The home page will display a list of classes and link to the survey page. This functionality is already there. For this setp just make sure everything is working and add any additional information and/or layout required. 
    
    > [!NOTE]  
    > At this point do not worry about the design. Just use basic HTML, black text, and Times New Roman. No CSS needed. 

7. **Survey Page**

    The survey page will provide three textareas to allow participants to answer the three questions. Once submitted the answers are saved to a database and a thankyou message is displayed. This functionality is alreay there. You can add any additional layout and/or functionality.

   You should add some basic server-side or client-side validation. 

9. **Console**

10. **Manage Classes**

11. **Results**
    
12. **Apply Design**

    Apply the designs from step one!

13. **About Colours** 

    Update the [colours-about](https://github.com/BrickMMO/colours-about) Markdown! Add your names to the `v2.markdown` page.

[&#10132; Back to V2](/colours-about/v2)

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
