# How to structure a project?
You're starting a new project. Be it scientific research, data science, taking an online course or even a study for commercial testing purposes, it will most likely involve some variations of these 3 parts:
1) input/starting information
2) analysis (exploring, testing, predicting, understanding)
3) communicate your findings to others (or document for yourself what you've learned).

It can also be simply a studying project where you take lessons (on-site or on-line) and apply what you learned in practical exercises. Then you will need to communicate to *your future self*. This is, when you come back to your course material 3 years from now, it would be nice to understand (instead of relying on remembering) what you intended to do, what you actually did, and why.

Yep, you'll need a reasonably robust structure for your files. The purpose of having a sensible folder/repo structure is to make the project easy to understand, maintain, and share with other people (including your future self).

In this particular case, I'm documenting the folder (or directory[^2]) /repository structure I use whenever I start a new project[^1].

I arrived at this structure after trying several different sources (see [here](03_output/documents/references/sources-inspiration) for some examples, but it is mostly shaped after modifications based on my own needs and many years of usage, both within and outside academia.

Still, this is a live, fluid idea, and is always subject to change: the 'best' structure (if that even exists) is the one that suits you better within a defined context, scope and purpose.

## Project template - overview

The folder structure looks like this:

```
Project-name
├── 01_data
│       ├── A_raw
│       └── B_processed
├── 02_code
│       ├── A_analysis
│       └── B_reports
├── 03_output
│       ├── A_figures
│       └── B_documents
│              └── references
│                       └── sources-inspiration.txt
└── readme.md
```

Yep. Nice and simple. (how do I get this representation after creating the tree structure?[^3]).
Note that the only two files are the current one you're reading (`readme.md`) and a text file with links to other references (`sources-inspiration.txt`), more on this later. (Why do I use XX_ and X_ in folder names?[^6]).

What you are reading right now is the famous _`readme.md`_ file, something like the 'landing page' of your project and/or repository.


Now let's go one by one.


## Structure breakdown

### `Project-name`
This is the *root* (aka *parent*) folder of your project. This is usually also the name of your remote repository (e.g. in GitHub). Everything (shareable) related to your project will live inside the `Project-name` folder. If you have to backup your project, usually you just need to copy this entire folder.

### `01_data`
I mostly use this as my 'input' folder. For example I set this location as the input path at the beginning of my code, where I set the 'global options' [^4]. In my experience, input data can be either `raw` or `processed`.

### `01_data/A_raw`
By 'raw' I mean data that you have been given, or obtained without doing any significant calculations or processing. I like to keep this separate because this is the 'original' data resulting from your research (or used as input to it) and it is data that you will never change in any upstream point of your workflow. If there is a single folder to save in case of computer/storage failure, this is probably the one (you can re-do all your analysis and reports, but in most cases it's not feasible to obtain your raw data again, especially in experimental sciences). You can only generate more raw data (e.g. new experiments, observations, data from other studies, etc.), but you don't change *existing* raw data[^5]. The worst case scenario is: you just don't use it. And if for some reason everything fails (I told you, I have experience! :d), these are the data you have to *come back to and start all over*. Examples of raw data: the survival of your animals after an experimental treatment, which include manually entered data (e.g. into a spreadsheet or lab notebook) as well as data collected by sensors / dataloggers that create their own data files (e.g. 1-minute interval temperature during the experiment). In most cases my data comes as CSV and NetCDF files. This folder also is the most detailed of all the input data you'll use. For example, you may have collected data of the temperature during your experiment at 1-minute intervals during 48 hours, but it was mostly to be sure the temperature was constant. In further analysis and reports, you will only use the main value and standard deviation, not the full 2880 values of temperature. So the CVS file with the 2880 values is *raw data*. After some preliminary analysis, you may want to use the standard error instead of the standard deviation in your further analysis. Thus you just change the *script that uses this data*, but you never change the temperature CVS file. In other words, raw data is the data that has not yet been subjected to any manipulation/modification after the experiment/observation.

In some cases (e.g. when I do GIS- or image-analysis related stuff), my raw data consists of otehr files such as raster (e.g. GeoTIFF, TIFF, PNG, etc.), vector drawings (e.g. SVG), GIS-related databases (GeoPackages, shapefiles, etc.), or video files (MP4, MOV, etc.) that are either sent to me by colleagues or downloaded from other sources. I consider them also 'raw' data because I take them as my input to extract information and I do not change them in any way.

### `01_data/B_processed`
After the explanation above, this is easier to explain. The processed data is also *input data* which started as 'raw' but has been already used in some way. By 'used' I mean it may have been re-structured or some calculations/numerical processing has been applied to it. Examples of re-structuring data involve the usual 'put all replicates into a single spreadsheet' case, where you copy-paste data from several CSV raw files into a single 'main' one, maybe also add columns for the treatments, change the name of the columns to better readability (e.g. `'O2[mg/L]'` instead of `'CHANNEL01'`), etc. Sometimes you need to calculate things from raw data to include in your processed data. For example for the temperature treatment, you may want to use the average of the 2880 values referred above, therefore the content of this new CSV file is obtained by processing the raw CSV from the temperature sensor. I try to create the 'processed data' using scripts (for easier reproducibility), but it may vary; some cases my not warrant a script if the processing is very simple.

### `02_code`
I put here all my scripts.

### `02_code/A_analysis`
The main coding activity of the project is done here. For me, this is the soul of my project (in terms of computer files). Here I have my (python, R, js, C, etc.) scripts. These scripts includes those that generate 'processed' data from 'raw' data, as well as those which use the processed data as input to perform further exploratory and inferential and predictive analysis (traditional hypothesis testing, Bayesian statistics, machine learning, etc.).

### `02_code/B_reports`
Here I place code that is not traditionally viewed as 'programming' but indeed uses scripting tools. One example is the use of [Quarto](https://quarto.org/) to create reports in different formats: the 'script' file is a markdown file (eg. `report.qmd`) that will render to a selected format such as PDF. The Quarto script can use other files as material (e.g. it may show a table of image from the `02_data` folder) but also can execute chunks of code (Python, R, js) and produce plots on the fly. A single Quarto report can also have different parts/chapter (you can use it to write an article or an entire book/thesis) which can be split into numerous separate Quarto markdown files. Therefore I personally prefer to have all these 'reporting' scrips in a separate 'reports' folder. It can be argued that the scripts to create reports should be placed into the `03_output/B_documents` folder (see below). But I prefer to use that one for the project's final, output documents, and not the code to generate them.

### `03_output/A_figures`
This is where I put all the figures generated by any of the scripts from the `02_code folder`. Not only any 'traditional' (scatter, bar, histogram) plots but also any other graph such as SVG, diagram, map (interactive or static). Imagine a report (book, thesis, article, talk) created from your project: if you would include something in there as a figure (and then reference it as Figure XX), then it should be in this folder. Or, if you would include an external file into your presentation (e.j. a bar plot exported as a PNG file), then it needs to be in this folder. This makes it easy to re-use for different purposes. Sure, sometimes it is not needed to have figures separately (for example in Quarto you can just create the figure when rendering your document). That is fine. The point of this folder is to have all figures (which are going to be needed elsewhere) in a sigle place (and hopefully with an appropriate filename), and avoid saving many times the same figure in different places and then not knowing which is the one you wanted to use (again, this never happened to me :p). The figures in this folders are mostly generated by the scripts in `02_code/A_analysis` folder, but it may happen that you generate the figure in one of your reporting scripts. I don't see a 'right' way here. Just a matter of personal preference.

### `03_output/B_documents`
If someone asks about your project, this would be the first place to send them. It has a *communication purpose*. I consider each file inside this folder as a self-contained document that will carry a message to a given audience. Here you include all the reports (or the only one, depending on our needs) of your project. Some are final deliverables, some may be intermediate documents.

For example you can have one or more of the following output documents:
- a presentation for a talk to showcase your project's plan / status / results.
- a report with a briefing for project management or updates for your company / institution.
- a deck, flyer, etc. intended for outreach and dissemination to non-specialists.
- a manuscript file (eg. a PDF with intro, figures, references, etc.) already formatted to be submitted to a serious academic journal[^7].
- eventually, the accepted manuscript to share with colleagues.

### `03_output/B_documents/references`
I place here the files with information about sources I have used overall in the project (e.g. websites, books, articles). The simplest form is a text file with a list of the URLs of your material. More 'advanced' cases include formatted files with citations created by some other programs or websites. Here you include all the citations you would need to replicate your study (data sources, papers, methods, other repositories, etc.). The text file can be used by the Quarto scripts (`reports` folder), with the appropriate format, and included as references such as bibliography-formatted list. The usefulness of this folder to me is probably related to my reading habits/workflow: whenever I find something (article, website, figure) related to any of my projects I want to write down the source (i.e. to cite it of even to explore it later). It is at this time that I need to go to a single well-known location and write down the source (most often the URL, but can also be a fully-formatted bibliographic reference). This way I avoid having all the sources scattered in several files that I may or may not find in the future! The bibliography can also be included in the report scripts, but I don't think it's a good practice as the references are not reusable and it is not easy to maintain (e. g. if one source changes then you have to may have to change it in several script files).

You may ask why not placing `references` in `02_code/B_reports` instead of output? The reason is that `references` is not something I create from input data, but mostly an element using to document the process, so I consider it as an output material, in this case a part of the `report`.

There is another use for this folder. Imagine you are preparing a presentation for a talk, and you want to include a [funny image](https://upload.wikimedia.org/wikipedia/en/3/3d/Dinosaur-diagram.gif) that was not generated by any of your scripts. If for whatever reason you want to include the image as 'local' (e.g. copy-paste into your presentation, or a path to a local file instead its URL), then you'll to download it to this folder. It is not a 'figure' crated by/for your project, so it doesn't belong in `03_output/A_figures`. It is also not a direct input for your project, thus it is also not raw data (it is not even data). It is a 'non-data' element that you use from a citable external source, so it goes into `references`.

In this current template repository I included the `sources-inspiration.txt` file, but in most cases I place here a `bibliography` file (normally generated by [Zotero](https://www.zotero.org/), but you can do it by hand if the list is not long) with a list of all the cited papers, books, websites, etc. used in my report documents.

## Add salt and pepper
That's it. This is what works for me, and I come back here to use the template every time I start a new project. This may even vary with each person mileage... You can use it as a guide, or completely modify it to your needs! You're welcome to contact me with ideas or suggestions!



[^1]: Note GitHub also has something called ['projects'](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects). This has nothing to do with the case discussed here.

[^2]: Directory = folder? No, but you don't have to worry about that now.
[^3]: The one I like the most (and used for this document is the [Text Tree Generator](https://www.text-tree-generator.com/). But there are a couple of other ways, see [here](https://stackoverflow.com/questions/1581559/ascii-library-for-creating-pretty-directory-trees) and [here](https://centerkey.com/tree/)
[^4]: Note to myself: link here to the python notebook template.
[^5]: There are cases when I am tempted to change raw data files. For example if I forgot to apply a calibration constant to the temperature data... it's just multiplying all my temperature values by a constant, right? what can go wrong? Well, since I tend to realize this kind of things way past the experiment, I try to handle these cases *in the script that uses the raw data* instead of changing the *raw data itself*. The reason is that I find it much more convenient and clear to explain what I did and why in a script than using an additional file next the the raw data file (or even worse: editing the raw data file which can change the date, file encoding, etc. of the original file). The other reason is that, if for some crazy mistake (that never happened to me, of course) the calibration constant was wrong, then I have to change the file again! Not a good practice...
[^6]: It's mostly for TOC-related organizational tendencies: in my file-managing setup, the directories are listed alphabetically, therefore by using those prefixes I see them ordered and nice, exactly as it is shown here. If I don't use the prefixes, the 'processed' filder will appear before 'raw', which is annyoing to me. *But what's with the leading zeroes in '01', '02', '03'? couldn't you just use '1', '2', '3'?* Yes, true. That is simply personal preference. Yes, I have a problem, I know :p.
[^7]: For example, check [Annals of Improbable Research](https://improbable.com/).
