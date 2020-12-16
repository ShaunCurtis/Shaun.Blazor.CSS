# Working with CSS in Blazor

This article describes how to customize the default CSS setup in Blazor, and looks at the new Scoped CSS.

It covers:

1. Customizing the deployed BootStrap.
2. How to switch to a different CSS Framework.
3. The new Scoped CSS functionality.

Please note that this article is aimed at programmers relatively new to DotNetCore and Blazor, and assumes you have some knowledge of SASS.  The article also assumes you're using Visual Studio 2019 - I use the Community Edition.

The code is avalable at [Blazor.CSS](https://github.com/ShaunCurtis/Blazor.CSS) on GitHub.

## Getting Started

1. Create a new Blazor Application using Net5.  I've used Server in the supplied code, but there's no difference between Server and WASM.
2. Run the site to make sure it works.
3. Install the [Web Compiler](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.WebCompiler)  extension.  *Extensions > Manage Extensions*.  You'll need to restart Visual Studio to complete the installation.

## Set up SASS

1. Add folder *SASS* to the project.
2. Move *wwwroot/css/site.css* file to *SASS* and rename it *custom.scss*.
3. Add *bootstrap-site.scss* to the folder.
4. Right mouse click on the file > Web Compiler > Compile File.
5. This will add a *compilerconfig.json* file to the project.  This is the configuration file for **Web Compiler**.

    *compilerconfig.json* will look like this:
```json
[
  {
    "outputFile": "SASS/bootstrap-site.css",
    "inputFile": "SASS/bootstrap-site.scss"
  }
]
```

6. Change this to output the compiled file into the web site:

```json
[
  {
    "outputFile": "wwwroot/css/site.css",
    "inputFile": "SASS/bootstrap-site.scss"
  }
]
```

7. A new *site.css* should appear in *wwwroot/css*.  There'll be nothing in it because the source file is empty.

## Set up Bootstrap

1. Add a *Bootstrap* folder to *SASS*. 
2. Add a *custom* folder to *Bootstrap*.
3. Download the Bootstrap Source from the Bootstrap site and copy the *scss* folder to *Bootstrap*.

    The full SASS folder (including the Spectre and other files that we will add later) should look like this:

![SASS folder](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/sass-folder.png?raw=true)

4. Edit *SASS/bootstrap-site.scss*.

```scss
/* Source SASS file to build custom Bootstrap site file */
@import "../wwwroot/css/open-iconic/font/css/open-iconic-bootstrap.min.css";
@import "Bootstrap/scss/_functions";
@import "Bootstrap/scss/bootstrap";
/* This is the original site.css file that contains the site specific customizations*/
@import "custom.scss";
```
5. Save and Web Compiler will compile a new site.css.  Watch the status in the bottom left corner of Visual Studio. 

    You should have a *site.css and a *site.min.css* in *wwwroot/css*.

6. Edit *_Host.cshtml*, and remove the reference to *bootstrap.min.css* - all the css is now compiled into *site.css*.

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor.CSS</title>
    <base href="~/" />
    \\ Remove bootstrap CSS reference
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    \\
    <link href="css/site.css" rel="stylesheet" />
    \\ This is the new Net5 build generated Scoped CSS stylesheet
    \\ more later in this article
    <link href="Blazor.CSS.styles.css" rel="stylesheet" />
</head>

```

7. Run the project.  Everything should be the same as before.

## Customize Bootstrap

We customize Bootstrap by adding new scss files.  I'm assuming you have a basic understanding of SASS.  If not then do a bit of background reading - it isn't rocket science.

To demonstrate customization, we'll adjust some of the colours and add some new button styles.  Most of the code is derived from the [SB2 Bootstrap template](https://startbootstrap.com/theme/sb-admin-2).

Add *_variables.scss* to *SASS/custom* and add the following content.  You can compare it with the base *SASS/Bootstrap/scss/_variables.scss* to see the differences.

```scss
// Override Bootstrap default variables here
// Do not edit any of the files in /bootstrap/scss/!

// Color Variables
// Bootstrap Color Overrides

$white: #fff !default;
$gray-100: #f8f9fc !default;
$gray-200: #eaecf4 !default;
$gray-300: #dddfeb !default;
$gray-400: #d1d3e2 !default;
$gray-500: #b7b9cc !default;
$gray-600: #858796 !default;
$gray-700: #6e707e !default;
$gray-800: #5a5c69 !default;
$gray-900: #3a3b45 !default;
$black: #000 !default;

// We've adjusted the colors
$blue: #4e73df !default;
$indigo: #6610f2 !default;
$purple: #6f42c1 !default;
$pink: #e83e8c !default;
$red: #e74a3b !default;
$orange: #fd7e14 !default;
$yellow: #f6c23e !default;
$green: #1cc88a !default;
$teal: #20c9a6 !default;
$cyan: #36b9cc !default;

$primary: $blue !default;
$secondary: $gray-600 !default;
$success: $green !default;
$info: $cyan !default;
$warning: $yellow !default;
$danger: $red !default;
$light: $gray-100 !default;
$dark: $gray-800 !default;
$brand: #b3ccff;

$theme-colors: () !default;
// stylelint-disable-next-line scss/dollar-variable-default

// We've added brand, add, edit,... 
$theme-colors: map-merge( ( 
    "primary": $primary, 
    "secondary": $secondary, 
    "success": $success, 
    "info": $info, 
    "warning": $warning, 
    "danger": $danger, 
    "error": $danger,
    "light": $light, 
    "dark": $dark,
    "brand": $brand,
    "add": $primary,
    "new": $info,
    "edit": $primary,
    "delete": $danger,
    "nav": $secondary,
    "change": $warning,
    "save": $success

), 
$theme-colors );

// Custom Colors
$brand-google: #ea4335 !default;
$brand-facebook: #3b5998 !default;

// Set Contrast Threshold
$yiq-contrasted-threshold: 195 !default;

// Typography
$body-color: $gray-600 !default;

$font-family-sans-serif: "Nunito", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", 'Noto Color Emoji' !default;

$font-size-base: .925rem !default; 
$font-size-lg: $font-size-base * 1.20 !default;
$font-size-sm: $font-size-base * .875 !default;

$font-weight-light: 300 !default;
// $font-weight-base: 400;
$headings-font-weight: 400 !default;

// Shadows
$box-shadow-sm: 0 0.125rem 0.25rem 0 rgba($gray-900, .2) !default;
$box-shadow: 0 0.15rem 1.75rem 0 rgba($gray-900, .15) !default;
// $box-shadow-lg: 0 1rem 3rem rgba($black, .175) !default;

// Borders Radius
$border-radius: 0.35rem !default;
$border-color: darken($gray-200, 2%) !default;

// Spacing Variables
// Change below variable if the height of the navbar changes
$topbar-base-height: 4.375rem !default;
// Change below variable to change the width of the sidenav
$sidebar-base-width: 14rem !default;
// Change below variable to change the width of the sidenav when collapsed
$sidebar-collapsed-width: 6.5rem !default;

// Card
$card-cap-bg: $gray-100 !default;
$card-border-color: $border-color !default;

// Adjust column spacing for symmetry
$spacer: 1rem !default;
$grid-gutter-width: $spacer * 1.5 !default;

// Transitions
$transition-collapse: height .15s ease !default;

// Dropdowns
$dropdown-font-size: 0.85rem !default;
$dropdown-border-color: $border-color !default;

/* turn off rounding */
$enable-rounded: false;
```

Add *_overrides.scss* to *SASS/custom* and add the following content.  This demonstrates the sort of changes you can make - we don't actually use it on the site.

```css
/* Reduce the default form-group bottom margin*/
.form-group {
    margin-bottom: .25rem;
}

/* set new margins and padding for small alerts*/
div.alert-sm .alert {
    padding: .25rem 1.25rem;
    margin-bottom: 0rem;
}
```

### Build the Customized Bootstrap

To build the custom version of Bootstrap, we need to add the new SASS files into the compile process.

Edit *SASS/bootstrap-site.scss*

```css
/* Source SASS file to build custom Bootstrap site file */
@import "../wwwroot/css/open-iconic/font/css/open-iconic-bootstrap.min.css";
@import "Bootstrap/scss/_functions";
@import "Bootstrap/Custom/_variables";
@import "Bootstrap/scss/bootstrap";
@import "Bootstrap/Custom/_overrides";
/* This is the original site.css file that contains the site specific customizations*/
@import "custom.scss";
```

Save and this should compile.

To see the changes, edit *Pages/Counter.razor* and add a few extra buttons to the page.

```html
<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
<button class="btn btn-save" @onclick="IncrementCount">Save Styled Click me</button>
<button class="btn btn-delete" @onclick="IncrementCount">Delete Styled Click me</button>
<button class="btn btn-brand" @onclick="IncrementCount">Brand Styled Click me</button>
```

Run the site and navigate to the counter page to check the button customization.  They should look like this:

![custom buttons](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/custom-buttons.png?raw=true)

## Changing CSS Frameworks

Not everyone wants to use Bootstrap - some people like their sites being a little different!  In this section we'll change to [Spectre](https://picturepan2.github.io/spectre/).

1. Download the Spectre code from [Github](https://github.com/picturepan2/spectre).
2. Create a *Spectre* directory in *SASS*.
3. Create a *scss* directory in *Spectre*.
4. Copy the contents of *spectre.src* into *SASS/Spectre/scss*.
5. Create *site-spectre.scss* in *SASS* and add the following code:

```scss
@import "Spectre/scss/spectre";
```

6. Edit *compilerconfig.json*

```json
[
  {
    "outputFile": "wwwroot/css/site.css",
    "inputFile": "SASS/site.scss"
  },
  {
    "outputFile": "wwwroot/css/spectre.css",
    "inputFile": "SASS/site-spectre.scss"
  },
  {
    "outputFile": "wwwroot/css/spectre-icons.css",
    "inputFile": "SASS/Spectre/scss/Spectre-icons.scss"
  }
]
```

Once you save this you should get a compiled *spectre.css in *wwwroot/css*

### Customization

Add folder *custom* to *SASS/Spectre*

Add *_variables.scss*

```scss
// Control colors
$brand-color: #7952b3 !default;
$exit-color: #66758c !default;
$save-color: #32b643 !default;
$delete-color: #e85600 !default;

```
Add *_buttons.scss*

```scss
// Button Colors
.btn {
    &.btn-exit {
        @include button-variant($exit-color);
    }
    &.btn-brand {
        @include button-variant($brand-color);
    }
    &.btn-delete {
        @include button-variant($delete-color);
    }
}
```

Edit *SASS/site-spectre.scss*

```scss
@import "Spectre/custom/_variables";
@import "Spectre/scss/spectre";
@import "Spectre/custom/_buttons";
```

Finally change *_Host.cshtml* over to the new css.

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor.CSS</title>
    <base href="~/" />
    \\ Link to the new custom Spectre CSS
    <link href="css/spectre.css" rel="stylesheet" />
    <link href="Blazor.CSS.styles.css" rel="stylesheet" />
</head>

```

Run the site.

It will look a little different - it needs some work to fix small problems, but basically works.  Go to the Counter page to see the different buttons - Spectre and Bootstrap button class names are very similar so work.

![spectre buttons](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/spectre-buttons.png?raw=true)


## Scoped CSS - Component Styling

A new feature in Net5 is scoped CSS a.k.a. component styling.  Take a look at the *Shared* folder in the project and you'll see css files attached to the *NavMenu* and *MainLayout* razor components.

![component css](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/component-css.png?raw=true)

To explore how this works, let's re-style the *FetchData* data table.

1. Add a new Razor component DataGrid.razor to *Shared*, and add the following code - it's a modified version of the existing *fetchdata* code.

```html
@using Blazor.CSS.Data

@if (forecasts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Temp. (F)</th>
                <th>Summary</th>
                <th class="max-column">Detail</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var forecast in forecasts)
            {
                <tr>
                    <td>@forecast.Date.ToShortDateString()</td>
                    <td>@forecast.TemperatureC</td>
                    <td>@forecast.TemperatureF</td>
                    <td>@forecast.Summary</td>
                    <td class="max-column">
                        <div class="grid-overflow">
                            <div class="grid-overflowinner">
                                @($"The Weather Forecast for this {forecast.Date.DayOfWeek}, the {forecast.Date.Day} of the month {forecast.Date.Month} in the year of our Lord {forecast.Date.Year} is {forecast.Summary}")
                            </div>
                        </div>
                    </td>
                </tr>
            }
        </tbody>
    </table>
}
@code {
    [Parameter] public WeatherForecast[] forecasts { get; set; } = null;
}
```

2. Add *DataGrid.razor.css* to *Shared*.  It should associate with *DataGrid.razor*.  The component specific css goes here.  We're adding some css to format the max column and handle text overflow gracefully.

```css
.max-column {
    width:50%;
}

.grid-overflow {
    display: flex;
}

.grid-overflowinner {
    flex: 1;
    width: 1px;
    overflow-x: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
```

3. Modify *Fetchdata* to use the new component.

```html
@page "/fetchdata"

@using Blazor.CSS.Data
@inject WeatherForecastService ForecastService

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from a service and a data grid to displaying it.</p>

<DataGrid forecasts="this.forecasts"></DataGrid>

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
    }
}
```

4. Run the site and *Fetchdata* should look like this:

![fatchdata](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/weather-datagrid.png?raw=true)

Note the max column operating with the ellipsis shortening as the page is narrowed.

### What's Going On

Open Developer Tools in the browser and take a look at the HTML.

![Elements](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/browser-elements-view.png?raw=true)

Note the new unique ID attribute used on various elements in the HTML.

Look at *Blazor.CSS.styles.css* - the CSS file generated by Blazor during the build process.  You can see the css scoped to the components using the unique IDs.

![sources](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/browser-sources.png?raw=true)

Finally, look at the *obj* view in Solution Explorer in the project.  You can see the specific css files generated for each component and the *bundled* file based on the project name.  This is exposed in the *wwwroot* folder. 

![obj-view](https://github.com/ShaunCurtis/Blazor.CSS/blob/master/images/obj-view.png?raw=true)

## Wrap Up

There are many ways to deploy and use custom CSS in Blazor.  This is one of many, but probably one of the simplest.

I've covered Scoped CSS here, though I'm not sure how much I'll use it.

