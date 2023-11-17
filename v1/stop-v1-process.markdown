<style>@import url("//readme.codeadam.ca/readme.css");</style>

## Process

This applciation includes a handful of tools to work with the LEGO® colour palette:

1. A colour directory including colour names and a sample colour.
2. A colour profile page including name, sample, alternative names, links to colours on other websites, and a sample brick, and other data.
3. A tool to enter a colour into a textbox and convert it to the closet LEGO® colour.
4. Can you brainstorm other colour palette tools?

There is no backend for this project. All backend functionality in completed by running the import script. Once deployed, this script will run once a month using [CRON](https://en.wikipedia.org/wiki/Cron). This will be setup by your instructor when the website is done and deployed. 

*** 

### Completed by Instructor

The [colours-v2](https://github.com/BrickMMO/colours-v2) includes a basic PHP core also used in [flow-v1](https://github.com/BrickMMO/flow-v2) and [stop-start-continue-v1](https://github.com/BrickMMO/stop-start-continue-v1).

There are two tables for this project named `colours`:

```sql
CREATE TABLE `colours` (
  `id` int(11) NOT NULL,
  `name` varchar(255) DEFAULT NULL,
  `rgb` varchar(255) DEFAULT NULL,
  `is_trans` enum('yes','no') DEFAULT NULL,
  `rebrickable_id` int(11) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

And externals:

```sql
CREATE TABLE `externals` (
  `id` int(11) NOT NULL,
  `name` varchar(255) DEFAULT NULL,
  `source` enum('brickowl','bricklink','ldraw','lego','peeron') DEFAULT NULL,
  `external_id` int(11) NOT NULL,
  `colour_id` int(11) NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

There is an import script that imports a list of colours from the [Rebrickable API](https://rebrickable.com/api/v3/docs/) into the `colours` and `externals` tables. This file is located at `/import.php`.

There is a home page named `index.php` that displays a list of the colours from the `colours` table.

*** 

### Steps

1. **Design**

    Using [Figma](https://www.figma.com/) (or your design tool of choice) mock up the application. The application should include three pages:

    1. **Home Page**

        A list of colours in a well presented format. Each colour could include the colour name, the RGB value, and/or a sample. Each colour should link to a profile page.

    2. **Profile Page**

        A page with all the details about a single colour. This could include the colour name, the RGB value, a sample of the colour, a sample Brick image, alternative names, and links to alternative sources (like [BrickLink](https://bricklink.com/), [BrickOwl](https://bricklink.com/), etc...). 

    3. **Conversion Page**

        A page that allows the visitor to enter a colour (name, RGB, or CMYK), and the form will display the closet colour(s) to the provided colour. The results should be displayed in a nice visual format.

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

    Using phpMyAdmin create a new database named `brickmmo_colours` and import the two SQL files in the `/imports` folder. You should now have two empty tables named `colours` and `externals`.

4. **Source Code**

    Clone the [colours-v2](https://github.com/BrickMMO/colours-v2) repo into your MAMP root folder (or point MAMP to your cloned directory). Restart the server if needed. 

    Copy the `/.env.sample` file amd name it `/.env`. Change the database variables to `brickmmo_colours` and the MAMP defaults:

    ```php
    DB_HOST=localhost
    DB_DATABASE=brickmmo_colours
    DB_USERNAME=root
    DB_PASSWORD=root
    ```

    At this point you can open open the BrickMMO Colours home page. It will be empty, but there should be no PHP errors. 

5. **Rebrickable API**

    Register for a [Rebrickable API](https://rebrickable.com/api/) key. Put the key ino the `.env` file:

    ```php
    REBRICKABLE_API_KEY=12345678901234567890123456789012
    ```

    Run the `import.php` script. This can be done by changing the URL to [http://localhost:8888/import.php](http://localhost:8888/import.php) on a Mac or [http://localhost/import.php](http://localhost/import.php] on a PC.

    Open up phpMyAdmin and your tables should now be full of records.

    > [!TIP]  
    > Steps six through thirteen have a project task in GitHub. Make sure to assign these tasks when starting. And then use these tasks to create your branch and a pull request when done.

6. **Home Page**
    
    Create the home page. Review the [Rebrickable API](https://rebrickable.com/api/v3/docs/) and the `colours` and `externals` tables to see what data is avaiable to put on the home page. Make these changes to the `/index.php` file. Don't worry too much about the design yet. 

    > [!NOTE]  
    > At this point do not worry about the design. Just use basic HTML, black text, and Times New Roman. No CSS needed. 

7. **Profile Page**

    On the home page you will link each colour to `/profile.php?id=1`. The number one will be replaced with the `id` value form the colours table. It will look something like this:

    ```php
    <?php while($colour = mysqli_fetch_assoc($result)): ?>

        <a href="profile.php?id=<?=$colour['id']?>"><?=$colour['name']?></a>
        
    <?php endwhile; ?>
    ```

    Create the a page named `profile.php`. Write the PHP and HTML to display a basic colour profile. Use the code in `/index.php` as a starting point. 

    Add some of the following data:

    - Colour Name
    - RGB value
    - Sample of the colour (there is an example of this in `/index.php`)
    - Links to alternative sources (like [BrickLink](https://bricklink.com/), [BrickOwl](https://bricklink.com/), etc...)

    You will need to write two `SELECT` queries to fetch as much information about the selected colour from the database. They will need to incorporate the `$_GET['id']` variable into the query. 

8. **Sample Brick Image**

    You can add a sample brick by using the [Rebrickable](https://rebrickable.com/downloads/) media. For example, the following URL:
    
    ```
    https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png
    ```

    Will display the following image:

    ![Red 3001 Brick](https://cdn.rebrickable.com/media/thumbs/parts/ldraw/4/3003.png/85x85p.png)

    The image URL consists of the following parts:

    - Rebrickable media URL: `https://cdn.rebrickable.com/media/thumbs/parts/ldraw/`
    - The colour ID using the `rebrickable_id` value from the `colours` table: `<?=$colour['rebrickable_id']?>`
    - The LEGO® brick ID using, you can look these up at the [LEGO® Pick-a-Brick Store](https://www.lego.com/en-ca/pick-and-build/pick-a-brick)
    - And the width and height of the image

9. **Conversion API**

    Create a folder named `/api`. All API files will be placed in this folder. 

    Create a file named `/api/convert.php`. For now you can test this file by visiting [http://localhost:8888/api/convert.php](http://localhost:8888/api/convert.php) on a Mac or [http://localhost/api/convert.php](http://localhost/api/convert.php] on a PC. At this point it will be an empty page.

    Copy the `include` statements from one of your other PHP files. 

    ```php
    <?php

    include('includes/connect.php');
    include('includes/config.php');
    include('includes/functions.php');
    ```

    You will need to adjust these include statements to reference files on folder above the `/api` folder. 

    Add `?colour=336699` to your URL. Just to test the colour URL variable, in the `/api/convert.php` file, output the colour provided in the URL. You will need to use `$_GET`.

    Add code that will convert the colour from the URL variableto the closest colour from the `colours` table. I would recommend incuding the three closest colours. **This is the hardest part!**

    Output the three colours using JSON.

10. **Conversion Tool**

    The last page needs to use the `/api/convert.php` file to convert a provided colour to the closet LEGO® pallette colour. This process will follow these steps:

    1. The visitor will enter a colour name, RGB, or CMYK value into the form.
    2. The visitor will click submit.
    3. JavaScript will make an API call to `/api/convert.php?colour=336699`. This API call will return a JSON list of the closest colours.
    4. JavaScript will then read the result and display the results. 

    Create a page named `/api/convert.php`. Add a link on the home page to this new page. 

    Add the form to this page. 

    Add JavaScript to validate the colour entered into the form. On submit make an API call to `/api/convert.php`. PParse the JSON response and display the top three colours. 

11. **Apply Design**

    Apply the designs from step one!

12. **About Colours** 

    Update the [colours-about](https://github.com/BrickMMO/colours-about) Markdown! Add your names to the `v2.markdown` page.

13. **Colours API**

    These additional API calls will not be needed for this application, but may be useful for other BrickMMO applications. 

    Create a file named `/api/colours.php`. This file will query all colours from the `colours` table and export them to JSON. Create a file named `/api/profile.php`. This file will need to be referenced as `/api/profile.php?id=1` and will return JSON of all available data for a single colour. 

[&#10132; Back to V2](/colours-about/v2)

---

<a href="https://brickmmo.com">
<img src="https://brickmmo.com/images/brickmmo-logo-horizontal.jpg" width="100">
</a>
