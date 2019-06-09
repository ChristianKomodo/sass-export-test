# Angular Sass File Export Test
### Testing what Sass files are exported with an Angular build.

A discussion arose as to whether unused Sass mixins and variables from imported files actually make it into the build, increasing the file size of the final build.  I decided to conduct a test.

For this test, two color variables were defined in `src/sass/variables/_colors.scss`:
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

## Results

After testing the resulting packages and files from `ng build` and `ng build --prod=true`, in both cases only the color value from `$variable-used` existed in the CSS of the final application build in the `/dist` folder.  The color from the `$variable-unused` was not exported and was not found in any files in the build.

Likewise, only classes defined in mixins that were actually used were present in the final build files in the application.  Unused mixins were not exported.

Therefore you can import as many Sass mixins and variables as you wish in any number of imported files and not affect the file size of the CSS in the final application.

This also means that importing Sass mixin or variable files individually, or importing an "index" file which in turn imports several other Sass files, have no impact on the final CSS when the application is built.
