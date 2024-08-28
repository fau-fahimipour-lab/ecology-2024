# Assignment 1

You may use any software to create your document (e.g., Microsoft Word, Google Docs, LaTeX, Markdown, etc.), but please submit your assignment as a PDF.

## Problem Set 1

The central theme of this course is that ecology studies the connections between different elements in nature. In this exercise, you will explore a peer-reviewed publication that proposes a hypothesis linking weather patterns to mosquito population dynamics. Building on this idea, you will conduct a simple data analysis to investigate further connections between weather patterns and mosquito-borne diseases in human populations.

### 1.1 Introduction

This exercise is based on the work of Jon Chase and Tiffany Knight (2003). For more details, refer to: *Drought-induced mosquito outbreaks in wetlands*. Ecology Letters, 6: 1017–1024. This reading is optional and available in the `Files` section on Canvas.
Chase and Knight propose that droughts lead to **increased** mosquito populations in the following year, particularly in wetlands that dry periodically (known as semi-permanent wetlands). This finding might seem counterintuitive, as one might expect wet years to be more beneficial for mosquitoes, providing more habitat for egg-laying. Mosquitoes typically lay their eggs in temporary water bodies, where larvae develop before emerging as adults.
However, the authors suggest that species interactions—specifically, predation and competition—play a key role in limiting larval mosquito populations in wetlands. They categorized wetlands into three types based on their drying frequency: permanent wetlands that never dry, temporary wetlands that dry every year, and semi-permanent wetlands that dry only during droughts.
In permanent wetlands, a variety of predators and competitors (e.g., mosquitofish) limit mosquito larvae survival. Temporary wetlands host competitors adapted to annual drying, such as zooplankton, which also limit mosquito larvae. In contrast, semi-permanent wetlands lack competitors and predators that can quickly recolonize after droughts. As a result, mosquitoes exploit the newly refilled wetlands, leading to a population spike before predators and competitors can reestablish.
One key connection discussed in Chapter 1 is between human activities and the spread of West Nile Virus (WNV). WNV can cause severe illness in humans and was first identified in the Congo in the 1930s. It has since spread globally, particularly in temperate and tropical regions. The first U.S. case was documented in New York in 1999. WNV is transmitted by infected mosquitoes (mainly of the genus *Culex*) when they bite humans.
Given the relationship between mosquito outbreaks and drought described in Chase and Knight's work, we hypothesize that WNV outbreaks might follow severe droughts. We will investigate this by comparing drought patterns to WNV incidence. The Palmer Drought Severity Index (PDSI) is a reliable measure of drought, combining temperature and precipitation data into a single index. Positive PDSI values indicate wet periods, while negative values indicate drought.
Your task is to test the relationship between drought and WNV incidence by plotting the mean PDSI (lagged by one year) on the x-axis and the number of reported WNV cases on the y-axis for various wetlands. "Lagged by one year" means that you will match the PDSI from one year to the number of WNV cases in the following year.

### 1.2 Performing a Simple Data Analysis

You may use any software for this data analysis, including Excel. Many biologists, both in academia and industry, use the `R` programming language for statistical and computational tasks. `R` can perform everything from basic calculations to advanced machine learning on large datasets. We will use `R` occasionally in this course for computations.
Fortunately, there are tools available to run `R` code directly in your web browser. While you can [install `R` locally](https://www.r-project.org/) on your computer, it is easier to use a web-based application. *Note:* I do not expect you to learn computer programming in this course. Instead, I expect you to [1] copy and paste code I provide into the web app, [2] customize the code slightly when necessary, [3] run the code, and [4] interpret the results.
Access the web app by visiting [https://rdrr.io/snippets/](https://rdrr.io/snippets/). You will see some example code in the console—delete it to start with a blank console. Then, copy and paste the code snippet below into the console. Do not run the code yet (*see below*).

```r
## Choose my custom color
myColor = "#000000"

## Input my data
droughtSeverity = c(-1.863, -1.525, 2.221, 4.504, 0.1, 1.177, 0.325, 0.243, 0.364, -0.419, 3.404, -0.136, 0.428, 0.736, -0.298, -1.273, 1.163)
westNileCases   = c(62, 237, 15, 25, 9, 10, 14, 0, 28, 6, 60, 11, 13, 30, 16, 20, 130)

## Plot my data and add a trendline
plot(westNileCases ~ droughtSeverity, pch = 16, cex = 2, bty = 'l', xlab = 'Drought severity index', ylab = 'West Nile cases')
abline(lm(westNileCases ~ droughtSeverity), col = myColor, lwd = 3)

## Calculate the slope of the trendline
cor(droughtSeverity, westNileCases)
```

**Before running the analysis**, customize your plot by selecting a unique color for your trendline. Visit [https://htmlcolorcodes.com/](https://htmlcolorcodes.com/) and use the sliders to choose a color you like. Copy the HEX code for your color and replace the code in `myColor = "#000000"` with your chosen color (inside the quotes). For example, if your color's hex code is #21674c, your line of code should read `myColor = "#21674c"`.

### Question 1.1

After selecting your custom color, run the code by clicking the `Run` button. Save your plot by right-clicking on it and record the slope of the trendline (the value output with your graph). Provide the hex code for your chosen color, the slope of the trendline, and your data figure.

### Question 1.2

Do you observe evidence of a link between severe drought and WNV incidence? Which features of your data support your argument? When do the highest WNV rates typically occur based on these data?

### Question 1.3

What might be a limitation of using the pattern observed by Chase and Knight to infer the incidence of mosquito-borne diseases?

## Problem Set 2

Create a document that highlights a plant and the biome it represents.
Go outside in your neighborhood, a favorite park, or another outdoor area with diverse plant life. Take pictures of a plant that represents the typical growth form of a biome you wish to feature (review these in Chapter 3). If you cannot access an area with different plant types, use [iNaturalist](https://www.inaturalist.org/) to look up plants in nearby areas.
Use the [interactive biome viewer](https://www.biointeractive.org/classroom-resources/biomeviewer) to find a place in the world that contains the biome you are featuring. Open the web page and click `Start Interactive`.
Different colors on the globe represent different biomes, as shown on the left side of the screen. Click and drag the globe to find a location with the biome you are highlighting, and click to drop a pin. A description of the place and its climatogram will appear on the right. **Take a screenshot** that includes the globe and all the information on the right side.

### Question 2.1

Provide a picture of your plant (make sure it is in focus and shows enough detail to highlight the growth form). You do not need to identify the species, but if you know what type of plant it is, feel free to include that information.

### Question 2.2

Name the biome you are featuring.

### Question 2.3

Describe the climatic features of the biome that contribute to the growth form (e.g., temperature and rainfall). Tell me your plant's growth form and highlight one feature in the plant picture that exemplifies this growth form.

### Question 2.4

Include the screenshot from the biome viewer of your chosen biome.
