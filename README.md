# Angular Sass File Export Test
### Testing what Sass files are exported with an Angular build.

A discussion arose as to whether unused Sass mixins and variables from imported files actually make it into the build, increasing the file size of the final build.  The [Sass documentation](https://sass-lang.com/documentation/at-rules/import#partials) states that those "Partials" do not compile on their own (see below) but I decided to conduct an experiment anyway.

> **Partials**  
> As a convention, Sass files that are only meant to be imported, not compiled on their own, begin with _ (as in _code.scss). These are called partials, and they tell Sass tools not to try to compile those files on their own. You can leave off the _ when importing a partial.

But let's see for ourselves.  For this test, two color variables were defined in `src/sass/variables/_colors.scss`:
```
$variable-used: #FE00FE;
$variable-unused: #0E6E6E;
```

In app.component.scss, we only actually use one of the variables in a defined class:
```
.class-used {
  color: $variable-used;
}
```

A color value was defined inside a mixin that was never used in order to determine if unused mixins made it into the final build:
```
@mixin font-size($size) {
  font-size: $size;
  font-size: calculateRem($size);
  color: #334455;  <--- color value to check for in final build
}
```

## Results

When observing the packages and files exported from `ng build` and `ng build --prod=true`, in both cases only the color value from `$variable-used` existed in the CSS of the final application build in the `/dist` folder.  The color from the `$variable-unused` was not exported and was not found in any files in the build.

Likewise, only classes defined in mixins that were actually used were present in the final build.  Unused mixins were not exported.  The color defined in the unused mixin was not found in the final build files.

**Therefore you can import as many Sass mixins and variables as you wish in any number of imported files and not affect the file size of the CSS in the final application.**

This also means that importing Sass mixin or variable files individually, or importing an "index" file which in turn imports several other Sass files, have no impact on the final CSS when the application is built.

### References

#### Sass Style Guide
https://sass-guidelin.es/#architecture
https://sass-guidelin.es/#main-file

#### Using Sass with the Angular CLI
https://scotch.io/tutorials/using-sass-with-the-angular-cli
