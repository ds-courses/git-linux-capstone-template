# Assignment git-linux-capstone

## Linux project description

You work with researchers interested in investigating changes in world happiness from year to year. Your task is to organize the data for a given year. You were provided data on country happiness from different institutes across six continents:
* Africa (AF)
* Asia (AS)
* North America (NA)
* South America (SA)
* Europe (EU)
* Oceania (OC)

To complete this task you will write a script to change the organization `continent_dirs/country_files` to `year_dir/continent_files` (cf below). This will require you to extract data for a given year from all countries and saving it in the appropriate `year/continent` file.

So for example if you extract all data corresponding to year 2015, your original data:

```
happiness_data
    ├─ AF              
    │   ├─ Algeria.csv
    │   ├─ Angola.csv
    │   └─ ...
    ├─ AS
    │   ├─ Afghanistan.csv
    │   ├─ Armenia.csv
    │   └─ ...
    ...
```
should become:
```
happiness_proj
    └─ preproc  
         └─ 2015
             ├─ AF_2015.csv
             ├─ AS_2015.csv
             ├─ ...
             └─ world_2015.csv
    
```
As you can see all data for a given year will be in a unique directory, with continents specified with the continent code in the filename. 

The data were recorded as text files, more precisely Comma Separated Value (CSV) files, i.e. each happiness metrics for a given year are separated by a comma (,). Each line in those files represents measurements for a different year. For example the content of the `happiness_data/AF/Angola.csv` file is:
```
Country name, year, Life Ladder, Log GDP per capita, ...
Angola, 2011, 5.589000701904297, 8.945781707763672, ...
Angola, 2012, 4.360249996185303, 8.991772651672363, ...
Angola, 2013, 3.9371068477630615, 9.004610061645508, ...
...
```
And the cleaned / preprocessed file `happiness_proj/preproc/2015/AF_2015.csv` will look like:
```
Country name, year, Life Ladder, Log GDP per capita, ...
Benin, 2015, 3.624664306640625, 7.988193511962891, ...
Botswana, 2015, 3.761964797973633, 9.608841896057129, ...
Burkina Faso, 2015, 4.4189300537109375, 7.562853813171387, ...
Cameroon, 2015, 5.037964820861816, 8.180439949035645, ...
```

To accomplish your data cleaning / preprocessing your script will rely on using the program `get_year` located inside the `bin` directory within your home dir (e.g. at `/home/ada/bin/get_year`) which extracts the row corresponding to a specific year.

Furthemore you were told to exclude datasets from countries with less than 5 time points (i.e. measurements for fewer than 5 different years) are those were deemed not suitable to study longitudinal changes.

## Git / GitHub

To validate part of the Git capstone project, you will have to use `git` to submit your work to Github. The steps are:
* check the link through Slack to connect to Github and select your course participant ID
* accept the assignment and refresh the page to get a link to your assignment Github repository
* clone your assignment repository on your computer
* follow the instructions below to complete each part of the Linux capstone project
* for each part create an annoted tag following the [semantic versioning convention](https://semver.org/), of the form `v0.M.P`, where:
  * M is a number indicating the minor version. For this project this will indicate the part number (e.g. `v0.2.P`), when submitting the code for part 2.
  * P is a number indicating the patch version.  For this project this will indicate your trial number (e.g. `v0.2.4`) when submitting your 4th trial to validate part 2.
* Check the Github `Actions` tab of your repository to see validation workflows, and then choose an autograding job:
  * `Autograding` for part 1
  * `Autograding part 2` for part 2
  * Etc.
* After clicking on a job, to see which tests were successful and which one failed, click on `Run neurorepro/autograding@v2`. You will see the name of the test next to the 📝 icon, and then a ✅ if it succeeded or a cross if it failed. At the end you will see a score:
  * The first part is graded over 10
  * The second part is graded over 20
  * Etc.
* You have to push your code with a tag for each part in order:
  * First do Part 1 and push with an appropriate tag (e.g. `v0.1.1`)
    * If it fails change your code, and push again with an incremental tag, e.g. `v0.1.2`, etc.
  * Then do Part 2 and push with an appropriate tag (e.g. `v0.2.1`)
  * Etc.
* Feel free to push code without tags to just update your code and files without launching the autograding
* **The name of the script should always be `preproc_year`** no matter the part you are working on: git is precisely use to track code changes, so no version in filenames is needed (or even wanted) 

Parts will be added as the tests get deployed. For now you can already work on part 1.


## PART 1
-----
Reorganizing the data and defining the script main variables.
1. Create a directory `happiness_proj` inside the `ada` directory

2. Copy recursively the content of `happiness_data` to a directory `raw` inside `happiness_proj` (note: `happiness_data` is in the `ada` directory). The results should be:
```
happiness_proj
    └─ raw
        ├─ AF              
        │   ├─ Algeria.csv
        │   ├─ Angola.csv
        │   └─ ...
        ├─ AS
        │   ├─ Afghanistan.csv
        │   ├─ Armenia.csv
        │   └─ ...
    ...
```

3. Create a new script called `preproc_year` inside the `happiness_proj` directory, that:
  * assigns to a variable `IN_DIR` the absolute path to your `happiness_proj` directory
  * assigns to a variable `RAW_DIR` the path to the `raw` directory, using the value of the `IN_DIR` variable previously defined
  * assigns to a variable `YEAR` an integer between 2008 and 2021 (choose it yourself)
  * assigns to an array `CSV_FILES` (do not forget quotes `"` `"`) all the CSV files which:
    * are in the subdirectories of `${RAW_DIR}`
    * ends with the `.csv` file extension

    TIP: find the right “globbing” expression and do not forget parentheses to define the array
  * **Within a SINGLE loop**, loop on each item of the resulting array `${CSV_FILES[@]}` and print:
    * the number of time points in that file
      * use `cat`, `|` and `wc -l` within a command expansion `$()`to save the number of lines in a temporary variable
      * use the arithmetic construct `$(( ))` to save the number time points in a variable `N_TIME_POINTS` (HINT: the previous temporary variable includes the header in its count, so you need to discount it)
      * display the value of `N_TIME_POINTS`
    * the name of the file (e.g. `Armenia.csv`) to save in a variable `COUNTRY_FILENAME` [TIP: use one string manipulation statement, based on the `#` symbol]
    * the name of the file parent directory (e.g. `AF`) to save in a variable `CONTINENT_CODE` [TIP: use two string manipulation statements: one based on the `%` symbol to create a temporary variable, and another one based on the `#` symbol applied to that temporary variable]

    NOTE: you only need one loop to do everything, so do not create more than one loop
  * Assuming your input directory is `/mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj`, the output should be as follows:
    ```
    /mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj/raw/AF/Algeria.csv
    10
    Algeria.csv
    AF
    /mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj/raw/AF/Angola.csv
    4
    Angola.csv
    AF
    /mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj/raw/AF/Benin.csv
    13
    Benin.csv
    AF
    /mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj/raw/AF/Botswana.csv
    12
    Botswana.csv
    AF
    /mnt/c/Users/me/i2ads/git-linux-capstone/root/home/ada/happiness_proj/raw/AF/Burkina Faso.csv
    15
    Burkina Faso.csv
    AF
    ...
    ``` 
    * If you find it useful, you can add any information / comment in the printed output as long as it starts with `=`, `-` or `#`. All other lines in the output should be exactly as above.

4. Make the script executable and run it. Use `shellcheck preproc_year` to check for errors

