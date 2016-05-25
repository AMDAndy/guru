### Drupal 8 Guru Theme

Guru is a gulp based Drupal 8 theme base with kss styleguide
 
### Features

- libsass for fast scss compilation
- browserSync for an amazing developing and testing experience
- singularity.gs css grid system
- KSS-Node to create stylesheets and maintain your style library
- Live Editing of scss in site and styleguide
- The styleguide enables you prototyping in SCSS with custom HTML in the comments section
- SCSS linting to drupal SCSS standards while live editing
- gulp as a build tool
- sourcemaps
- autoprefixer


Checkout presentation at Drupalcamp Essen 2015
http://drupalcamp-essen.de/15/session/creating-a-gulp-based-d8-theme-with-browsersync

See the slides
http://slides.com/digitaldonkey/create-a-drupal8-theme

Please use drupal issue tracker to provide feedback and share improvements. 
https://www.drupal.org/project/issues/search/2599668


### Dependencies

**Drupal Modules**

You  **need the drupal module link_css** to make browserSync working!

[link_css](https://www.drupal.org/project/link_css)

Here is the reason:
https://github.com/BrowserSync/browser-sync/issues/10

**Other dependencies**

- npm, bower
- libsass
-  ~~Ruby~~ Just for the second scss-lint option now.


### Getting started

**Create a subtheme**
You may rename the theme according to your needs or create a subtheme with drush.
Make sure using drush8. Lullabot tells you [how to use multiple drush versions](https://www.lullabot.com/articles/switching-drush-versions).
 
```
drush guru "My Subtheme" 
```

**Guru theme setup**

1) You should have bower and npm available in your command line.
If your grid system choice will be singularity you need bower.
```
npm -v
3.8.3
bower -v
1.6.5
```

2) Install node dependencies
```
cd [theme folder]
npm install --global gulp bower browser-sync
npm install 
```

3) Install singularity (Currently 1.7.0) and compass-sass-mixins
```
cd [theme folder]
bower install
```

4) **Change your domain and styleguide url** in
```
[theme folder]/gulp.config.js
```

5) Run gulp in theme folder
```
cd [theme folder]
gulp
```

6) Enable theme in Drupal
and check the styleguide in /themes/custom/[theme folder]/styleguide/




### Readme


##### KSS-Node
Is a amazing tool to create living styleguides.

Look into the scss files for examples. 

The styleguide is generated into 
/themes/custom/[your theme]/styleguide/
and should open on the browser using the gulp default task.

In order to only compile the styleguide use `gulp styleguide`

You may change the templates in 
/themes/custom/[your theme]/styleguide-template/

For more information: 
https://github.com/kss-node/kss-node/wiki/Quick-Start-Guide
https://www.npmjs.com/package/kss
https://github.com/kss-node/kss/blob/spec/SPEC.md

Listen to [John Albin Wilkins at Drupalcon LA 2015](https://www.youtube.com/watch?v=y5coJloNutU) in order to learn why you want this. 

#### Libsass
is a much faster implementation compared to the slow ruby SASS.
http://sass-lang.com/libsass

**Installing libsass with homebrew**
```
brew install libsass
```

There are some differences between compass-sass and lib-sass left.
For details read here:
http://www.sitepoint.com/switching-ruby-sass-libsass/

Spriting with libsass is not working yet.
But maybe this https://github.com/wellington/wellington

#### Singularity
You may know singularity from Omega4 theme.  

Read the [introduction](https://github.com/at-import/Singularity) or the extensive [docs](https://github.com/at-import/Singularity) 

#### SCSS-lint or sass-lint

Linters for SCSS files help you to write cleaner code. Errors are logged to the console id a linter is enabled. 


**disabled (default)**

No Linting. You may change settings in gulp.config.js in order to use the following options.

**sass-lint**
In sass-lint it is currently not possible to [disabling linters by comment](https://github.com/sasstools/sass-lint/issues/70). 
As soon as https://github.com/sasstools/sass-lint/pull/677 is committed to sass-lint I will drop scss-lint support.  

The sass-lint configuration is in sasslint-drupal.yml 

**scss-lint**
https://github.com/brigade/scss-lint

This sadly now depends on ruby. 

```
gem install scss_lint
```
The linter's configuration is in scsslint-drupal.yml 

Using scss-lint there is "disabling by comment" enabled.

```
// scss-lint:disable ImportantRule
```

    
#### Digging deeper

In order to understand the underlying gulp processesing you should have a look in the gulpfile.js. There you find all gulp tasks and can figure out what they do.

Check out package.json and bower.json if you want to know more about about all packages involved. 

Guru is using drupal default css classes provided by core classy theme. 
You can change this by changing the "base theme" variable in theme.info.yml
At [lulabot](https://www.lullabot.com/articles/a-tale-of-two-base-themes-in-drupal-8-core) you can read about Drupal 8 base themes.


### HOWTO: Getting started in Drupals Vagrant VM (Debian/Ubuntu) 

*Testet with the [Vagrant Drupal Development](https://www.drupal.org/node/2008792) Virtual machine.*

Get Drupal 8 running and drush8 running all together the most easy way is using a virtual machine created with [Vagrant Drupal Development](https://www.drupal.org/node/2008792). 

I will assume you have made it so far. 

**Install the latest node-js in Ubuntu**

Do you have node installed? 

```
npm -v
3.3.6
```

If not:

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Install global packages**

```
npm install --global gulp
npm install --global browser-sync
npm install --global bower
```

I had to fix permission Problems, when trying to do npm install --global in the vm.
And [found a fix](http://stackoverflow.com/a/21712034/308533): 

```
npm config set prefix '~/.npm-packages'
```

and add

```
export PATH="$PATH:$HOME/.npm-packages/bin"
```

to the end of you ~/.bashrc


**Testing bower**

```
bower -v
1.6.5
```


**Create Subtheme**

```
cd [www-data-folder]
drush guru "My Theme"
```

**Local node modules:**

```
cd [theme folder]
npm install 
bower install singularity
```

**Adapt your config** in
 
```
gulp.config.js
```

In my VM I Ended up using:

```
domain: 'drupal8.dev',

styleguide: {
    uri: 'http://drupal8.dev:3000/themes/my_theme/styleguide/index.html'
  },
```

**Don't forget to install link_css module**

```
drush dl
drush en link_css -y
```

Enable the Theme in Drupal UI or with Drush

```
drush pm-enable my_theme -y
drush config-set system.theme default my_theme -y
```


**Run gulp and open up a browser**
 
```
cd [theme folder]
gulp
```

If you are on a remote/Vagrant computer browser want start up. 

You can use the Urls provided in "External" you should find after browSersync Init is finished.
