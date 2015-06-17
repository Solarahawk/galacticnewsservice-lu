#The Galactic News Service for Litcube's Universe
v.1.0 (adapted from v.1.05 of The X-Tended Galactic News System pdf)

> Credit for the Galactic News Service - LU Edition goes to the XTC team who developed and released the original GNS plugin for modders. Many thanks to everyone on that team for their amazing work, and for **enenra** granting me permission to release this updated package.

**From the original XT GNS Guide:**

> Welcome to the GNS. As many people in X3: Terran Conflict missed the old BBS-System from X3:Reunion, we have developed a new system, which allows news to be available to the player in a similar fashion as X3:Reunion.

This version of the GNS - LU Edition has been updated specifically to provide Litcube's Universe with a central news service. This is primarily a modder's resource, but anyone can install it right away and see random static news articles around the universe. GNS is intended to allow other LU plugins, such as the Anarkis Pirate Guild and the EMS missions system to publish news-worthy events. This will add color and life to the universe by allowing you to hear about interesting, real-universe events happening as a result of these plugins, and one day, you might even read about your own deeds and growing fame in the universe.

As mentioned, the GNS does come with a set of static news articles that it will randomly display when you open your local News Console. These are primarly to add a small bit of color and fill in the gaps when there isn't much dynamic news to report, but be aware that these articles were imported by the XTC team from X3: Reunion. Thus, in this initial version, there is a chance some news items may pop up that are a bit anachronistic for LU (doesn't match the updated universe in LU). My primary focus is on creating dynamic content that can be published to the News Service, but if I have time, I will see about reviewing the static news channel for any possible updates or improvements that it might need. Anyone is VERY welcome to taking a look at the t-file 9500-L044.xml and providing me with an updated news list (inlcuding additional or different tags). Contact me on the Egosoft forums, if you have suggestions or updates to send me.

##Changes from XT GNS plugin:

The primary change in this updated plugin is the original News Console has been significantly rewritten in order to use LU's Dynamic Menus framework. This not only improves the responsiveness of the News Console, so there is no flicker, but dynamic articles automatically appear and disappear as they are added to the news service or when they expire. I also have some rough ideas for extending the new dynamic News Console to provide other centralized information services, such as an improved comm system and bulletin board for the Anarkis Guilds (to be announced in the future.)

Other changes to this plugin involved removing legacy code for the XTC mod, and various tweaks for LU.


##Installation:

Excluding the PDF which is not needed, copy the addon\ folder in the archive into your X3 root directory. The GNS plugin is automatically available in your game once it is installed.

For modders (or anyone curious to see what dynamic news articles can look like in their game), a second folder is provided in the archive, named "ForModders". It contains a testing/tutorial plugin named "GNS-DynamicNewsGenerator". This is an AL plugin that, when enabled, will begin generating dummy news articles for the game. In the default setup, a new article is generated once a minute, and it is randomly published with a number of different characteristics.

* Note that not all articles will be published for your current sector or region. If you move around the galaxy, you may see news articles that were not visible in other locations.

To install the news generator plugin, copy its addon\ folder to your X3 root directory, and in the game, enable the plugin in the Gameplay-> Artifical Life Settings menu. To uninstall, the plugin, first disable it in the AL menu. When the plugin displays the message box stating it has been disabled, save your game and quit. Then remove the plugin files from the addon\ folder.


##Start a new game or load a saved game.
###For players:
To open a newspaper you will need to define a hot-key in the interface section of LU’s control setup options menu. After this has been done, you will just need to press that key for a newspaper to open. The system will open a newspaper that will be based on the owner of the sector you are in (if a newspaper is available). The selection of static articles will be random within the available pool of articles that are available to that race.

The usage of the plugin should be self-explaining: click on an article to view it. There is also an option to save articles to a personal clipboard for later reading.

###For scripters:

The activation of the Show.Newspaper script can be done from any place you would like, not just via the hotkey. This has been deliberately done to make this system as flexible as possible.

###The included scripts:
- setup.plugin.GalacticNewsService – setup-script, executed at every gamestart 
- plugin.GNS.Add.Article – adds dynamic news article to the service 
- plugin.GNS.Add.NewsFeed – adds a custom newsfeed to the to the static news channels. Basically, allows you to add your own set of custom static news items. 
- plugin.GNS.GetID – returns a free and unique id for dynamic news
- plugin.GNS.Remove.Article – removes a dynamic newsarticle from the system 
- plugin.GNS.Remove.NewsFeed – removes an own newsfile from the newsfilelist
- plugin.GNS.Setup.Newspapers – reads out all available static newsfeeds 
- plugin.GNS.Uninstall – uninstalls the script
- Menu.GNS.Show.Newspaper – connects to GNS console (the main menu)
- Menu.GNS.Show.Newspaper.SM - Set Menu script for Menu.GNS.Show.Newspaper (part of the LU Dynamic Menu framework)
- Menu.GNS.Show.Clipboard - GNS Console submenu for displaying articles saved to the clipboard
- Menu.GNS.Show.Clipboard.SM - GNS clipboard Set Menu


###The system itself:
There are two possible types of news this system can support, static news and dynamic news:
1. **Static News:**
These are articles that, as the name suggests, are static and work in a similar fashion to the old X3: Reunion news articles. These have topic and race tags to define when and where they will be shown, and will be picked at random based on these conditions when a newspaper is opened.
2. **Dynamic News:**
The GNS plugin currently provides no dynamic news articles, as the current system is a direct port from X3: Reunion, which did not support dynamic news. The purpose of this plugin is to make it easy for modders to publish dynamic news from their own plugins.

Do you want to add a news article that praises the player for good deeds after the completion of a mission? Do you want a news article only to show after something specific in your mod has happened? Well with the dynamic news system this is possible.

To publish dynamic news into the system, we will use a script to add the article to the system (or to remove it): Call scripts/plugin.GNS.Add.Article with these arguments:
- **Identifier type="Var/String"** – this is a unique identifier, so you can later remove the article (with the remove-script), if you wish. You can get a unique, free id by using the script **plugin.GNS.GetID**.
-**Sector.or.Race type= "Value"** – here you can specify the sector or race in which (sectors) the article can be read . This can have different formats:
    1. A given object (from MD) can specify the sector, an array of objects an array of sectors.
    2. A sector or an array of sectors
    3. A race as DATATYPE_RACE
    4. A race as a string, case-sensitive (Argon,Teladi), (Boron,Split,Terran) 
    5. An array of races
    6. null or 0 as an argument will publish the article everywhere
- **Article.Title type="Var/String"** – the title of the article, which is shown in the system
- **Article.Body type="Var/String"** – the article itself, which is shown when selected
- **Priority type="Var/Number"** – not currently used
- **Show.Once type="Var/Boolean"** – if set, this article can be shown only once, otherwise the article must have an expiration time (see next arg) or you have to remove it by the remove-script and your identifier. MD can use integer (0/1). An article flagged for Show.Once will be available until the Console is closed, at which time it is removed.
- **Duration type="Var/Number"** - this is the duration (in seconds) that the article can be shown in the system. After this duration, the article will be removed. MD can use integer
- **Script.Name type="Var/String"** – this is the name of the action script, that will be executed when the action button (see below) is pressed. Alternately, it is possible for an article to autmoatically execute this script by setting this argument value, but not passing any button arguments. The action script will only be executed once, whether by button-press or automatically.
- **Button.Headline type="Var/String"** – this is the text for the headline directly above the action button 
- **Button.Text type="Var/String"** – this is the text label for the button

###The T-file:

There are two pages: 9500 and 9501 in the main newsfile 9500. You can add additional custom newsfeeds (t-files) using plugin.GNS.Add.NewsFeed, giving it the new t-filenumber.

**9500 (or your own newsfile first page) is the static news articles and their tags:**

- From t-id 10 to whatever are the possible races, this can be changed only in 9500, not in your custom newsfiles 
- t-id 30 is the number of possible topics (tags)
- From t-id 31 to whatever are the possible topics (tags)
- t-id 99 is the number of static articles available
- t-id 100 is the tagging of the first article in the format: topics:race, topics separated by “,” race starts with “:”
- t-id 101 is the title of this article
- t-id 102 is the article body

* further article t-ids are 100 + number*10
* further article title t-ids are 100 + number*10 + 1 
* further articles body t-ids are 100 + number*10 +2

**9501 (or your own newsfile second page) are the newspapers and their tags:**

- t-id 01 is the number of all newspapers
- t-id 100*number are the races of the newspaper
- t-id 100*number + 1 is the title of this newspaper
- t-id 100*number + 2 is the coverpage of this newspaper
- t-id 100*number + 9 is the number of topics inside this newspaper 
- from t-id 100*number + 10 to whatever are the topics, that this newspaper has, articles are taken randomly from the static news, when the topics allow this.

The tag system has been designed to allow you to define how and when a news article will display. Each t-id that used for tags is in two sections. The first section must always at least 1 tag, as these define which article types the news is assigned to (an article has to have at least one topic.) The second section is used to define which races can and cannot display this article. This can be left blank if you wish the news to be available to every race.

###A list of the current tags :
**Topic Tags (all can be replaced or changed to whatever you like, but the articles have to fit to your topics):**
- sports 
- social
- trade 
- science 
- military 
- crime 
- galnet 
- argon
- boron
- pirate
- split 
- paranid 
- teladi 
- racing 
- review 
- catastrophe 
- goner 
- accident 
- politics 
- archaeology 
- advert 
- xenon 
- humour 
- terran 
- phanon
- business

**Race Tags (these can be changed for your mod, if you want to use other races):**
- Argon 
- Boron 
- Split 
- Paranid 
- Teladi 
- Pirates 
- Goner 
- Terran 
- ATF
- Phanon
- OCV

Race tags can be inverted to define a block on a race. Instead of allowing a particular race, deny it by adding a ! on the front of the tag. For example instead of using 'Argon' to allow the article to be shown in Argon space, you can use '!Argon' to block that article from showing in Argon space. Normal and inverted tags cannot be mixed in the same t-id though, so think how you wish to achieve your race definitions for an article. Tags are also case sensitive.

Topic and Race tags are divided using a : and the individual tags for each section are divided with a ,

An example for clarification: A crime, military, and pirate article available to the Boron, Argon and Teladi would look like this:

    crime,military,pirate:Boron,Argon,Teladi 

Note there are no spaces.

If an article does not need race tags (like a Galnet article for example), then you do not include the colon in the tag.

    crime,military,pirate

Tags in each section can be in any order, but race tags must always be after topic tags.

If you need further clarification on tag usage, please look at the tagging in the included txt file (9500- L044.xml) in the t folder.

##Appendix A:
There are some placeholder-variables, which are automatically replaced, if you use them in your article bodies. This works for both static and dynamic news articles (but not the titles!).

Here is a list of possible variables (should be self-explanatory). The variables used in your text will automatically be replaced with the requested content. For dynamic articles, this text replacement occurs at the time of adding the article to the News Service. Random variables will not change between different viewings of a given article:

    $PLAYERNAME$
    $PLAYERSHIPCLASS$
    $PLAYERSHIPTYPE$
    $PLAYERSHIPID$
    $PLAYERSHIPNAME$
    $PLAYERSECTOR$

    $RANDOMNAME$
    $RANDOMNAME.ARGON$
    $RANDOMNAME.BORON$
    $RANDOMNAME.SPLIT$
    $RANDOMNAME.PARANID$
    $RANDOMNAME.TELADI$
    $RANDOMNAME.TERRAN$
    $RANDOMCREDITS1$ - replaces string with number from 1,000- 100,000
    $RANDOMCREDITS2$ - replaces string with number from 100,000 – 100,000,000

    $RANDOMSECTOR$
    $RANDOMSECTOR.ARGON$
    $RANDOMSECTOR.BORON$
    $RANDOMSECTOR.SPLIT$
    $RANDOMSECTOR.PARANID$
    $RANDOMSECTOR.TELADI$
    $RANDOMSECTOR.XENON$
    $RANDOMSECTOR.OCV$
    $RANDOMSECTOR.PHANON$
    $RANDOMSECTOR.PIRATES$
    $RANDOMSECTOR.GONER$
    $RANDOMSECTOR.ATF$
    $RANDOMSECTOR.TERRAN$
    $RANDOMSECTOR.YAKI$