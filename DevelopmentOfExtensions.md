
# Extending Asterisk and Drupal #

Some info on extending Asterisk and Drupal functionalities by creating modules and such.

# Extending Drupal #

## Installing new modules ##

Drupal modules usually ship as a tar.gz archive which we have to extract to the drupal's /modules/ folder (for example: /var/www/drupal/modules). After extraction, go to the drupal administration interface (log in as an administrator user and go to Administer), go to Site building then Modules and chose to "run cron manually" if necessary or just observe that new modules have been installed.

For each module, we can enable or disable features by ticking checkboxes. Although, some features require other modules to be installed first. This is the case of the

## File Framework ##
The File Framework module helps to upload, download, remove, preview, convert files through the Drupal interface. It needs the RDF and Bitcache modules to enabled.

### Installation ###
The **fileframework module** can be downloaded from [this page](http://drupal.org/project/fileframework/). Download the latest/available version, it should be ok.

Install the **[RDF module](http://drupal.org/project/rdf/)** and the **[Bitcache module](http://drupal.org/project/bitcache)** (Important, **don't download alpha-3 see (1)** !).

(1) Either download the alpha2 version (from december 2008) or the last development version (from 14 january 2009 or later), but not alpha-3 which is buggy and won't allow you to create any new piece of content ! Use [this link to be able to download more versions](http://drupal.org/node/192590/release) of the Bitcache module.

### Configuration of the File browser view ###
In order to have the file browser up, in Drupal's interface, **enable the "Browser" feature** in Administer > Site building > Modules > File management.

**The File Browser exposes a block** which you can put you can put in a region. See the Enabling installed modules blocks below.

## Taxonomy DHTML menu ##
The [Taxonomy DHTML module](http://drupal.org/project/taxonomy_dhtml) which depends on the [DHTML Menu module](http://drupal.org/project/dhtml_menu) exposes a block that shows the taxonomies tree with a limited or unlimited depth.

The DHTML component of both modules allows the generated menus/trees to be expanded on click without forcing a full page reload and also give nice animation effects when menu items are expanded/collapsed.

### Installation ###
The taxonomy\_dhtml module just requires the dhtml\_menu module to be installed. See the above installing modules section if you don't know how to install them.

### Configuration ###
Once the two above-mentioned modules are installed and enabled, configuration for the Taxonomy DHTML module can be done in Administer > Site configuration > Taxonomy DHTML. There, it is important to check the "Expose block" option and why not to increase the Depth to some bigger number or "All" so as to be able to see the full tree. You will likely need to move the newly exposed block from this Taxonomy DHTML module to a page region thanks to the Blocks administration view (see the "Enabling installed modules blocks" below).

## Drupal blocks - enabling blocks for installed modules ##

In Drupal, this is possible to let modules or information be located on left,right,top and bottom (footer) regions/panes/sidebars of all or specific pages. The sort of widgets/information that modules can put in these regions on request is called blocks.

Once some module is enabled (built-in ones are automatically enabled), go to Administer > Site building > Blocks. Then go to the "Disabled" section to see blocks that have not been assigned a region yet, and for the block you want to place somewhere change the drop-down box which is `<none>` to something such as "Left sidebar".

Be sure to click "Save blocks" at the bottom of the page to submit changes.

There's a also a nice [video tutorial on blocks here](http://blip.tv/file/222725).

# Extending Asterisk #

## Sounds ##

### Adding new sounds to Asterisk ###
On [this encyclopedia page](http://encyclopedia.wordpress.com/2007/09/25/asterisk-writing-native-extensions-for-asterisk-12/), Justin Tunney says:
"The core encoding method for Asterisk is AST\_FORMAT\_SLINEAR, in which non-compressed audio is sampled 8000 times a second, with 16-bit signed samples." (those files in .sln extension)

For conversion into .gsm (natively accepted by Asterisk): once you have some files to convert, you should follow the tutorial at [this page](http://www.voip-info.org/tiki-index.php?page=Convert+WAV+audio+files+for+use+in+Asterisk) to do roughly the following things:
  1. Convert your sound files to .gsm or .sln file format using sox or whatever software application you want.(1)
  1. Move your files to /usr/share/asterisk/sounds/ (and inside another folder of your choice if you want).
  1. In extensions.conf (from /etc/asterisk/) you Playback() and BackGround() calls should refer to your file name's path relative to /usr/share/asterisk/sounds/ and without any extension (no .gsm or .wav if ever you manage to make .wav played).

(1) note: If you use sox, the `sox source.wav dest.gsm` command without more options may not be sufficient to give the right results because the sampling rate is obviously unchanged between source and destination. If that sampling rate is too high, Asterisk will play the gsm at less than the speed you want.

## Dialplan (extensions.conf) ##

### What is a context ? ###
**In Asterisk's extensions.conf** and any file included by it, a **_context_** is a section of code starting with a header like `[someContextName]` and containing extension lines (ie. ` exten => ....`-like lines).

### Coding new functions to be used in the dialplan ###
_All quoted stuff comes from [this page](http://encyclopedia.wordpress.com/2007/09/25/asterisk-writing-native-extensions-for-asterisk-12/) written by Justin Tunney._
"The following directories in the Asterisk source tree are important and worth mentioning."

  * "funcs/ Where custom dial plan function modules are stored. Custom functions are registered with ast\_custom\_function\_register()."

### Including contexts and external files ###
From any context, it is possible to **include** ("make a copy/paste") **some other context's code from the current file** by writing:
```
[someContextName] #somewhere in the file
...
[anotherContext]
include => someContextName
```

It is also possible to **include the contents of another extensions file** using the `#include` statement followed by another dialplan/extensions filename without any surrounding quotes. For example:
```
#include AudioSpreadExtensions.conf
```