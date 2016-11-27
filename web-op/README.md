## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started

#### Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

#### Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>

### Student's Write Up for Part 1
#### Hosting The Site Using ngrok for PageSpeed Insights Check

1. Download/clone the submitted file.
1. Check out the repository
1. Run a local server

  ```bash
  $> cd /path/to/dist
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. [ngrok](https://ngrok.com/) is already downloaded to the top-level of the dist directory. Run the following command to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/dist
  $> ./ngrok http 8080
  ```
1. Paste the Forwarding link to Google's [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) for checking.

#### Optimization Done for Part 1
1. Minified HTML, JS, and CSS. See Gruntfile.js and package.json for Grunt task runner settings.
2. Optimized images. This is also done via Grunt with ImageMagick.
3. Removed render-blocking-css by inlining css.
4. Use `async` to asynchronously download and execute perfmatters.js and analytics.js as they don't alter css.
5. Moved inline JavaScript to above CSS to avoid blocking of CSSOM.

#### Build Tools - Grunt
1. Grunt is used to execute tasks such as minify HTML, JavaScript, CSS and optimize images.
1. If you haven't installed npm, the Node.js package manager installed and/or Grunt, follow the [instructions](http://gruntjs.com/getting-started) here to get started.
1. Once you have npm and Grunt installed in the project directory, follow the instructions in the links below to install:
  * [grunt-responsive-images](https://github.com/andismith/grunt-responsive-images) - for image optimization.
  * [htmlmin](https://github.com/gruntjs/grunt-contrib-htmlmin) - for minifying HTML
  * [uglify](https://github.com/gruntjs/grunt-contrib-uglify) - for minifying JavaScript
  * [cssmin](https://github.com/gruntjs/grunt-contrib-cssmin) - for minifying CSS
1. Edit Gruntfile.js and package.json if necessary.

### Student's Write Up for Part 2
#### Summary
1. To check the performance, open `path/to/views/pizza.html` on Google Chrome.
1. All changes/optimizations made are done in `views/main.js`.

#### Optimization Done for Part 2
1. For changing the size of pizza (under the function `changePizzaSizes` at line 429), the determineDx function is removed as it is unnecessary to access the window width to determine the size of the pizzas element. This can be done by specifying percentage of the container's width as per changePizzaSizes function below.
1. For moving pizzas as the page scrolls (under function `updatePositions` at line 491), originally document.body.scrollTop is called at each step of the loop, as style.left is called afterwards, this was causing forced synchronous layout.
Hence scrollTop which is used to calculate pizza element phase is moved out of the loop with declaration of a new variable called scollTop, FSL is therefore avoided.
1. Using backfaceVisibility to force the browser to render each pizza element in its own layer. This allows the browser to repaint the pizza more efficiently.
There is a tradeoff between the time required to paint vs. time required for scripting and rendering - as there are now more layers to create, it will take more time for scripting  and rendering to complete.
In this case the gain in Painting is greater than the loss in Scripting and Rendering, hence this approach is used.
