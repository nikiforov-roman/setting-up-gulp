# Setting up Gulp 4
Modern web development has many repetitive tasks like running a local server, minifying code, optimizing images, preprocessing CSS and more. This text discusses gulp, a build tool for automating these tasks
## 1. Install NodeJS from the official site
`https://nodejs.org/en/`
## 2. Check for node, npm, and npx
`node --version`  
`npm --version`  
`npx --version`  
## 3. Install the Gulp command line utility
`npm install --global gulp-cli`
## 4. Create a new project
Create a project directory and navigate into it.
## 5. Create a package.json file in your project directory
`npm init`  
This will guide you through giving your project a name, version, description, etc.
## 6. Install the Gulp package in your devDependencies
`npm install --save-dev gulp`
## 7. Verify your gulp versions
`gulp --version`
## 8. Create a gulpfile
Using your text editor, create a file named **gulpfile.js** in your project root with these contents:
```
let project_folder = require("path").basename(__dirname);
let source_folder = "src"
 
let path = {
    build: {
        html: project_folder + "/",
        css: project_folder + "/css/",
        js: project_folder + "/js/",
        img: project_folder + "/img/",
        fonts: project_folder + "/fonts/",
    },
    src: {
        html: [source_folder + "/*.html", "!" + source_folder + "/_*.html"],
        css: source_folder + "/scss/style.scss",
        js: source_folder + "/js/script.js",
        img: source_folder + "/img/**/*.{jpg,png,svg,gif,ico,webp}",
        fonts: source_folder + "/fonts/*.ttf",
    },
    watch: {
        html: source_folder + "/**/*.html",
        css: source_folder + "/scss/**/*.scss",
        js: source_folder + "/js/**/*.js",
        img: source_folder + "/img/**/*.{jpg,png,svg,gif,ico,webp}",
    },
    clean: "./" + project_folder + "/"
}
 
let {src, dest} = require('gulp'),
    gulp = require('gulp'),
    browsersync = require("browser-sync").create(),
    fileinclude = require("gulp-file-include"),
    del = require("del"),
    scss = require("gulp-sass"),
    autoprefixer = require("gulp-autoprefixer"),
    group_media = require("gulp-group-css-media-queries"),
    clean_css = require("gulp-clean-css"),
    rename = require("gulp-rename"),
    uglify = require("gulp-uglify-es").default,
    imagemin = require("gulp-imagemin"),
    webp = require("gulp-webp"),
    webphtml = require("gulp-webp-html"),
    webpcss = require("gulp-webpcss"),
    svgSprite = require("gulp-svg-sprite");
 
function browserSync(params) {
    browsersync.init({
        server: {
            baseDir: "./" + project_folder + "/"
        },
        port: 3000,
        notify: false
    })
}
 
function html() {
    return src(path.src.html)
        .pipe(fileinclude())
        .pipe(webphtml())
        .pipe(dest(path.build.html))
        .pipe(browsersync.stream())
}

function css() {
    return src(path.src.css)
        .pipe(
            scss({
                outputStyle: "expanded";
            })
        )
        .pipe(
            group_media()
        )
        .pipe(
            autoprefixer({
                overrideBrowserslist: ["last 5 versions"],
                cascade: true,
            })
        )
        .pipe(webpcss())
        .pipe(dest(path.build.css))
        .pipe(clean_css())
        .pipe(
            rename({
                extname: ".min.css"
            })
        )
        .pipe(dest(path.build.css))
        .pipe(browsersync.stream())
}

function js() {
    return src(path.src.js)
        .pipe(fileinclude())
        .pipe(dest(path.build.js))
        .pipe(
            uglify()
        )
        .pipe(
            rename({
                extname: ".min.js"
            })
        )
        .pipe(dest(path.build.js))
        .pipe(browsersync.stream())
}

function images() {
    return src(path.src.img)
        .pipe(
            webp({
                quality: 70
            })
        )
        .pipe(dest(path.build.img))
        .pipe(src(path.src.img))
        .pipe(
            imagemin({
                progressive: true;
                svgoPlugins: [{ removeViewBox: false }],
                interlaced: true,
                optimizationLevel: 3 // 0 to 7
            })
        )
        .pipe(dest(path.build.img))
        .pipe(browsersync.stream())
}

gulp.task('svgSprite', function() {
    return gulp.src([source_folder + '/iconsprite/*.svg'])
        .pipe(svgSprite({
            mode: {
                stack: {
                    sprite: "../icons/icons.svg",
                    // example: true
                }
            },
        }))
        .pipe(dest(path.build.img))
})
 
function watchFiles(params) {
    gulp.watch([path.watch.html], html);
    gulp.watch([path.watch.css], css);
    gulp.watch([path.watch.js], js);
    gulp.watch([path.watch.img], images);
}
 
function clean(params) {
    return del(path.clean);
}
 
let build = gulp.series(clean, gulp.parallel(js, css, html, images));
 
let watch = gulp.parallel(build, watchFiles, browserSync);

exports.images = images; 
exports.js = js; 
exports.css = css;
exports.html = html;
exports.build = build;
exports.watch = watch;
exports.default = watch;
```
## 9. Create a project structure
In the root directory of the project, create a folder **src** to store the source files. Create 4 more folders inside the crk folder: **img**, **scss**, **js**, **fonts**. Here we will create a file **index.html**. Create a **style.scss** file in the **scss** folder. Create a **script.js** file in the **js** folder.
## 10. Installing the required plugins
`npm i browser-sync gulp-file-include del gulp-sass gulp-autoprefixer gulp-group-css-media-queries gulp-clean-css gulp-rename gulp-uglify-es gulp-imagemin gulp-webp gulp-webp-html gulp-webpcss gulp-svg-sprite --save-dev`