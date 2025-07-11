ggplot2

ggplot2, like dplyr, is also one of the core packages of the tidyverse. It is useful for creating complex and aesthetically pleasing visualizations. Once you know how to use ggplot2, you will never need to use GraphPad or Excel again for your plots!

ggplot2 follows the grammar of graphics, a systematic way of building plots by specifying distinct components/layers of a plot:

Data: The dataset we want to visualize. This could be one you have just cleaned up with dplyr.

Aesthetics (aes): Mappings of variables in the dataset to visual properties of the plot like x, y, color, size, etc.

Geometric objects (geom): Types of plot layers, like points, lines, bars, etc.

Other components: Additional elements such as facets and themes.

{r}
library(tidyverse)

📊 My Understanding of ggplot2 and the Grammar of Graphics (Formatted with ChatGPT)

When I first started using ggplot2, it immediately felt intuitive — almost like second nature. Coming from a background in GIS mapping, Photoshop, and Illustrator, I was already used to thinking in terms of layers, visual hierarchy, and attribute-based styling. In GIS, I often stack raster and vector layers with symbology tied to attribute fields; in Photoshop or Illustrator, I work with layer effects, transparency, and masks. ggplot2 felt like the data visualization equivalent of that design thinking.

🎨 What is ggplot2?

ggplot2 is a data visualization package in R created by Hadley Wickham, based on Leland Wilkinson’s Grammar of Graphics — a conceptual framework that treats graphics as layered and composable systems.

The beauty of ggplot2 lies in its simplicity and consistency. Each visual you create is constructed layer by layer using the + operator, where each layer adds something — points, lines, labels, themes, etc.

📐 TL;DR – Grammar of Graphics

Wilkinson’s Grammar of Graphics breaks a plot into the following components:

Data: The dataset you're working with.

Aesthetics (aes): How variables are visually mapped (x, y, color, size, etc.).

Geoms: The geometric objects like points, bars, or lines.

Stats: Statistical summaries or transformations.

Scales: Control how data values are visually represented.

Coords: Coordinate systems (Cartesian, polar, etc.).

Facets: Split the plot into panels by group.

Theme: Control non-data visual elements like text, background, gridlines.

This modular system is powerful. You're not limited to choosing a chart type — you're assembling a graphic based on what you want to show.

🔁 Why ggplot2? Where is ggplot1?

Interestingly, ggplot2 wasn't the first attempt. Hadley Wickham initially created ggplot (or ggplot1) as a prototype. It implemented the Grammar of Graphics but was limited in functionality and flexibility.

Then came ggplot2, a complete rewrite and redesign — hence the “2” in the name. It introduced a more polished and extensible system, with clearer syntax, more control, and better performance. ggplot1 is no longer maintained, and most users today only know ggplot2.

🧠 Why ggplot2 Makes Sense to Me

Using ggplot2 feels like working in GIS or vector design tools:

I map data to visuals using aes() — just like mapping fields to symbology.

I add visual layers with geom_*() — like adding shapefiles or vector layers.

I control layout with facet_wrap() — much like using layout panels or map grids.

I tweak appearance using theme() — just like applying design styles in Illustrator.

This way of building a plot by layering information is deeply intuitive to me. It’s not just making a chart; it’s constructing a data-driven visual narrative.

{r}
print(ChickWeight)

🐣 Dataset: ChickWeight

Type: Longitudinal growth data

Use: Measures the growth (weight gain) of chicks under different diet treatments over time.

🧾 Codebook (Variable Descriptions)

Variable

Type

Description

weight

Numeric

Body weight of the chick in grams

Time

Numeric

Age of the chick in days since birth

Chick

Factor

Unique ID for each chick (1–50); identifies individual subjects

Diet

Factor

Diet group assigned to the chick (1–4); represents different feeding regimens

💡 Key Facts:

The dataset follows 50 chicks over 21 days.

Chicks are randomly assigned to 4 different diets.

Weight is measured repeatedly over time for each chick (i.e., longitudinal/repeated measures design).

Chick, Diet are subject-level variables, while Time and weight are observation-level variables (i.e., repeated measurements).

The dataset is commonly used to analyze growth curves, treatment effects, and mixed models.

Scatterplot

ggplot(data = ChickWeight): This initializes the plot with the dataset ChickWeight.

aes(x = Time, y = weight): In ggplot(), The aes() function specifies our mappings. Here, Time is mapped to the x-axis and weight is mapped to the y-axis.

geom_point(): This adds the geometric layer that plots the data as points, making it a scatterplot.

{r}
# ggplot(data = ChickWeight, aes(x = Time, y = weight)) + 
#  geom_point()

# I prefer this one
# ChickWeight %>% 
#  ggplot(aes(x=Time, y = weight)) +
#  geom_point()

# I don't like the fact that the column names are capitalized.
# Let's rename the columns using dplyr

cw <- ChickWeight %>% 
  rename_with(tolower)

# I prefer this one
cw %>% 
  ggplot(aes(x=time, y = weight)) +
  geom_point() # geom_point basically means scatterplot in ggplot2 syntax


color = Diet: In aes() we map the color of the points to Diet.

size = 3: In geom_point(), we set the size of the points to make them larger than the default.

labs(): Here, we can specify a title for the plot and labels for the x-axis and y-axis.

Don't hesitate to play with these elements to get a feel for how it changes the plot.

{r}
# ggplot(data = ChickWeight, aes(x = Time, y = weight, color = Diet)) + 
#  geom_point(size = 3) + 
#  labs(title = "Chick Weight Gain over Time by Diet", x = "Time", y = "Weight")

# I prefer this:

cw %>% 
  ggplot(aes(x = time, y = weight, color = diet)) +
  geom_point(size = 3) +
  labs(  #this adds labels/caption
    title = "Chick Weight Gain over Time by Diet",
    x = "Time",
    y = "Weight"
  )


Boxplot

geom_boxplot(): This creates a boxplot showing the weight distribution for each value of Diet.

{r}
# ggplot(data = ChickWeight, aes(x = Diet, y = weight)) + 
#  geom_boxplot()


#The basic structure is 1) data + 2) aes() + 3)geometry (geom) + other optionals

# My preferred style
cw %>% 
  ggplot(aes(x = diet, y = weight)) +
  geom_boxplot()


geom_jitter(): This adds a layer on top of the boxplot that displays data points with slight random variation, or "jitter".

width = 0.2: In geom_jitter(), this specifies how much "jitter" to apply to the points. This means that each point will be moved randomly left or right by a small amount, up to 0.2 units. This helps spread out points that might otherwise overlap, making it easier to see each point.

color = "orange": In geom_jitter(), this sets the color of the points to orange.

alpha = 0.5: In geom_jitter(), this sets the transparency level of the points. An alpha value of 0.5 makes the points semi-transparent, reducing visual clutter when points overlap. The value can range from 0 (fully transparent) to 1 (fully opaque).

Don't hesitate to play with these elements to get a feel for how it changes the plot. What additional information is gained by adding jitter?

{r}
cw %>% 
  ggplot(aes(x = diet, y = weight)) + 
  geom_boxplot() + 
  geom_jitter(width = 0.3, color = "blue", alpha = 0.3) + # I am changing the width and alpha according to my preference
  labs(
    title = "Chick Weight by Diet",
    x = "diet",
    y = "weight"
  )

Facets

We can use facet_grid() to split our data into subplots based on categorical variables. This allows us to see interactions between variables.

facet_grid(. ~ Diet): Creates separate plots for each value of the Diet variable. Each value of Diet is a column.

facet_grid(Diet ~ .): Creates separate plots for each value of the Diet variable. Each value of Diet is a row.

{r}
# ggplot(data = ChickWeight, aes(x = Time, y = weight)) +
#   geom_point() +
#   facet_grid(. ~ Diet)
# 
# ggplot(data = ChickWeight, aes(x = Time, y = weight)) +
#   geom_point() +
#   facet_grid(Diet ~ .)

# "Ctrl + Shift + C" to comment multiple lines
# "Ctrl + Shift + M" to insert pipe (%>%)
# "Alt + -" to insert "<-" 

cw %>%
  ggplot(aes(x = time, y = weight)) +
  geom_point() +
  facet_grid(. ~ diet)

cw %>%
   ggplot(aes(x = time, y = weight)) +
   geom_point() +
   facet_grid(diet ~ .)


If there was another categorical variable in ChickWeight, called X, we can then use facet_grid(X ~ Diet) to create a grid of plots with rows representing X and columns representing Diet.

Themes

ggplot2 comes with several built-in themes that can give your plots different looks. You can use theme_minimal(), theme_classic(), etc., to change the appearance of your plot.

{r}
# ggplot(data = ChickWeight, aes(x = Time, y = weight, color = Diet)) + 
#   geom_point(size = 3) + 
#   labs(title = "Chick Weight Gain over Time by Diet", x = "Time", y = "Weight") +
#   theme_classic()


cw %>% 
  ggplot(aes(x = time, y = weight, color = diet)) + 
  geom_point(size = 3) + 
  labs(title = "Chick Weight Gain over Time by Diet", x = "Time", y = "Weight") +
  theme_classic()


Exercises

Indometh contains pharmacokinetic data from an experiment that measured plasma concentrations of the drug indomethacin over time after IV (original text said oral) administration to six volunteers (after single intake). Indomethacin is a nonsteroidal anti-inflammatory drug (NSAID) used to treat mild to moderate acute pain and relieve symptoms of arthritis or gout.

{r}
print(Indometh)

💊 Dataset: Indometh

Name: Indometh

Source: Built into the datasets package (or from nlme)

Type: Pharmacokinetic data

Use: Measures the blood concentration of indomethacin over time after a single intravenous dose.

🧾 Codebook (Variable Descriptions)

Variable

Type

Description

Subject

Factor

Unique ID of the subject (1–6); each row belongs to a specific individual

time

Numeric

Time since drug administration, in hours

conc

Numeric

Plasma concentration of indomethacin, in mcg/mL

💡 Key Facts:

The dataset contains data from 6 subjects.

Each subject received a singel IV dose of indomethacin (a non-steroidal anti-inflammatory drug).

Plasma concentration was measured at multiple time points after administration.

This is longitudinal pharmacokinetic data — each subject has repeated measurements.

Commonly used for nonlinear modeling of drug elimination (e.g., exponential decay).

Make a scatterplot that visualizes the relationship between time and conc.

{r}
# Your code here
im <- Indometh

im %>% 
  ggplot(aes(x=time, y= conc)) +
  geom_point()

Make a boxplot that visualizes the conc distribution for each Subject.

{r}
# Your code here
im %>%
  ggplot(aes(x=Subject, y= conc)) +
  geom_boxplot() +
  geom_jitter(width = 0.3, color = 'red', alpha = 0.8) +
  labs(title = "Concentration over time for each subject",
       x="Subject",
       y="Concentration")

# desired_order <- c("1", "2", "3","4", "5", "6")  # example
# im %>%
#   mutate(Subject = factor(Subject, levels = desired_order)) %>% 
#   ggplot(aes(x = Subject, y = conc)) +
#   geom_boxplot() +
#   geom_jitter(width = 0.3, color = 'red', alpha = 0.8) +
#   labs(title = "Concentration for each subject", 
#        x = "Subject", 
#        y = "Concentration")




Make a plot with facets that visualizes the relationship between time and conc for each Subject.

{r}
# Your code here
im %>%
  ggplot(aes(x = time, y = conc)) +
  geom_point() +
  facet_grid(. ~ Subject)
  #facet_grid(Subject ~ .)

# im %>% 
#   ggplot(aes(x = time, y = conc)) +
#   geom_point() +
#   geom_line() +  # optional: adds lines connecting points for each subject
#   facet_wrap(~ Subject) +
#   labs(
#     title = "Concentration over Time by Subject",
#     x = "Time",
#     y = "Concentration"
#   ) +
#   theme_minimal()



Theoph contains pharmacokinetic data from a study that measured plasma concentrations of the drug theophylline after administration. Theophylline is used to treat respiratory diseases like asthma.

🧾 Codebook (Variable Descriptions)

Variable

Type

Description

Subject

Factor

Unique ID of the subject (individual patient); 12 levels (12 subjects)

Wt

Numeric

Weight of the subject in kg

Dose

Numeric

Dose of theophylline given, in mg/kg

Time

Numeric

Time after drug administration, in hours

conc

Numeric

Theophylline concentration in blood plasma, in mg/L

💡 Key Facts:

Each subject received a single dose.

Blood samples were collected at multiple time points to measure drug concentration.

The dataset is often used for nonlinear pharmacokinetics, modeling drug absorption and elimination.

Subject, Wt, and Dose are subject-level variables, while Time and conc are observation-level (i.e., repeated measures for each subject).

{r}
print(Theoph)

Make a scatterplot that visualizes the relationship between Time and conc.

{r}
# Your code here

th <- Theoph %>% 
  rename_with(tolower)

# colnames(th)

th %>% 
  ggplot(aes(x = time, y = conc)) +
  geom_point() + 
  labs(title = "Concentration of theophylline over time", 
       x = "Time", 
       y = "Concentration")

Make a boxplot that visualizes the conc distribution for each Dose. If Dose is not yet a factor, you will have to use factor(Dose).

{r}
# Your code here
th %>% 
  ggplot(aes(x = factor(dose), y = conc)) +
  geom_boxplot()


Make a boxplot that visualizes the conc distribution for each Subject. If Subject is not yet a factor, you will have to use factor(Subject).

{r}
# Your code here
th %>% 
  ggplot(aes(x = factor(subject), y = conc)) +
  geom_boxplot()

Make a plot with facets that shows how the relationship between Time and conc differs for each Subject-Dose combination.

{r}
# Your code here.
th %>% 
  ggplot(aes(x = time, y = conc)) +
  geom_point() +
  geom_line() +
  facet_grid(dose ~ subject) +
  labs(
    title = "Theophylline Concentration Over Time by Subject and Dose",
    x = "Time",
    y = "Concentration"
  )


th %>%
  ggplot(aes(x = time, y = conc)) +
  geom_point() +
  geom_line() +
  facet_wrap(~ subject + dose) +
  labs(
    title = "Theophylline Concentration Over Time by Subject and Dose",
    x = "Time",
    y = "Concentration"
  )

esoph contains data from a study on the relationship between esophageal cancer and several risk factors, such as alcohol and tobacco consumption. The study aims to investigate how these risk factors may contribute to the occurrence of esophageal cancer.

🧬 Dataset: esoph

Type: Epidemiological case-control study data

Use: Analyzes the relationship between alcohol and tobacco consumption and the risk of esophageal cancer.

🧾 Codebook (Variable Descriptions)

Variable

Type

Description

agegp

Factor

Age group of subjects (6 groups: 25–34, 35–44, ..., 75+)

alcgp

Factor

Alcohol consumption group (4 levels: 0–39g, 40–79g, 80–119g, 120g+)

tobgp

Factor

Tobacco consumption group (4 levels: 0–9g, 10–19g, 20–29g, 30g+)

ncases

Integer

Number of cases (people with esophageal cancer) in that group

ncontrols

Integer

Number of controls (people without cancer) in that group

💡 Key Facts:

This is a case-control study from a retrospective survey in France.

Data are aggregated, not individual-level — each row represents a group defined by age, alcohol, and tobacco categories.

The goal is to evaluate how alcohol and tobacco use (separately and jointly) are associated with esophageal cancer risk.

You can compute odds ratios, fit logistic regression, or visualize dose-response patterns.

{r}
print(esoph)

# unique(esoph$alcgp) # how many catergories, 4 category
# unique(esoph$tobgp) # 4 category

Make plot to show which agegp has the highest frequency of esophageal cases. Then do the same for alcgp and tobgp. Hint: Frequency of esophageal cases has not been calculated yet in esoph.

{r}
# Your code here, answer key at the end

# esoph %>% 
#   group_by(agegp) %>% 
#   summarize(freq_agegp = sum(ncases)) %>% 
#   ggplot(aes(x = agegp, y = freq_agegp)) +
#   geom_col() +
#   labs(
#     title = "Total Esophageal Cancer Cases by Age Group",
#     x = "Age Group",
#     y = "Number of Cases") +
#   theme_minimal()

# esoph %>% 
#   group_by(alcgp) %>% 
#   summarize(freq_alcgp = sum(ncases)) %>% 
#   ggplot(aes(x = alcgp, y = freq_alcgp)) +
#   geom_col() +
#   labs(
#     title = "Total Esophageal Cancer Cases by Alcohol Consumption Group",
#     x = "Alcohol Consumption Group",
#     y = "Number of Cases") +
#   theme_minimal()

# esoph %>% 
#   group_by(tobgp) %>% 
#   summarize(freq_tobgp = sum(ncases)) %>% 
#   ggplot(aes(x = tobgp, y = freq_tobgp)) +
#   geom_col() +
#   labs(
#     title = "Total Esophageal Cancer Cases by Tobacco Consumption Group",
#     x = "Tobacco Consumption Group",
#     y = "Number of Cases") +
#   theme_minimal()

I had problem with understanding the difference between geom_bar and geom_col. I thought geom_bar would give me the result. Here the difference between geom_bar vs geom_col.

📊 geom_bar() vs geom_col() in ggplot2

Feature

geom_bar()

geom_col()

Use Case

When your data is not yet summarized

When your data is already summarized

Stat by default

stat = "count"

stat = "identity"

Y-axis meaning

Counts the number of rows per category

Uses the actual y-values in your data

Common mistake

Used on summarized data (won’t show correct bar height)

Used on raw data (won’t work unless y is defined)

Y aesthetic

Usually omitted (y is calculated)

Must provide y = value

🧠 Quick Rule of Thumb

Question

Answer

Do you want ggplot2 to count for you?

Use geom_bar()

Have you already counted/summarized the data?

Use geom_col()

Based on the three separate plots you made above, which alcgp-tobgp combination would you say carries the lowest risk for having esophageal cancer? What about highest risk? Make a plot with facets to see if your prediction is right.

{r}
# Your code here, answer key at the end



Would you say that alcgp and tobgp interact to predict esophageal cancer risk? How does agegp factor into this?

{r}
# Your written response here


Exploratory Data Analysis

I have emphasized mostly scatterplots and boxplots with facets because, honestly, if you are good at applying these visualizations, you are well on your way to performing exploratory analysis of biological data.

Answer Key

{r}
# Make plot to show which agegp has the highest frequency of esophageal cases. Then do the same for alcgp and tobgp. Hint: Frequency of esophageal cases has not been calculated yet in esoph.

esoph_mutated <- esoph %>%
  mutate(npeople = ncases + ncontrols) %>%
  mutate(esoph_freq = ncases / npeople)

ggplot(data = esoph_mutated, aes(x = agegp, y = esoph_freq)) +
  geom_boxplot() +
  geom_jitter(color = "orange", alpha = 0.5)

ggplot(data = esoph_mutated, aes(x = alcgp, y = esoph_freq)) +
  geom_boxplot() +
  geom_jitter(color = "orange", alpha = 0.5)

ggplot(data = esoph_mutated, aes(x = tobgp, y = esoph_freq)) +
  geom_boxplot() +
  geom_jitter(color = "orange", alpha = 0.5)

# Based on the three separate plots you made above, which alcgp-tobgp combination would you say carries the lowest risk for having esophageal cancer? What about highest risk? Make a plot with facets to see if your prediction is right.

ggplot(data = esoph_mutated, aes(x = agegp, y = esoph_freq)) +
  geom_point() +
  facet_grid(tobgp ~ alcgp)

At first, I thought the question was simply asking which age group, alcohol group, or tobacco group had the highest number of esophageal cancer cases. So I grouped the data by each variable and summed up ncases, then used geom_col() to visualize the totals. My logic was straightforward: higher total cases must mean higher frequency, right?

But then I saw the answer key and realized I had misunderstood what "frequency" meant in this context.

What I had calculated was absolute counts — the raw number of cases in each group — but the question was actually asking for relative frequency or risk: that is, out of all the people in each group (cases + controls), what proportion had cancer?

The key difference is that some groups might have more people overall, which naturally leads to more cases even if the risk per person is lower. So my method didn't account for the size of each group. In contrast, the correct approach calculated the frequency as a proportion (ncases / (ncases + ncontrols)) and used a boxplot to show how this varied within each group.

{r}
# Let's do it my way
ec <- esoph %>% 
  mutate(ec_freq = ncases / (ncases + ncontrols)) #calculated the proportion/freq

print(ec)

# Now,I can calculate freq based on agegp
ec %>% 
  ggplot(aes(x = agegp, y = ec_freq)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, color = "red", alpha = 0.5) +
  theme_minimal() +
  labs(title = "Frequency of esophageal cancer based on age group",
      x = "Age Group",
      y = "Frequency"
      )

# now for alcgp
ec %>% 
  ggplot(aes(x = alcgp, y = ec_freq)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, color = "red", alpha = 0.5) +
  theme_minimal() +
  labs(title = "Frequency of esophageal cancer based on Alcohol consumption",
      x = "Alcohol consumption",
      y = "Frequency"
      )

# now for tobgp
ec %>% 
  ggplot(aes(x = tobgp, y = ec_freq)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, color = "red", alpha = 0.5) +
  theme_minimal() +
  labs(title = "Frequency of esophageal cancer based on Tobacco consumption",
      x = "Tobacco consumption",
      y = "Frequency"
      )


{r}
# Based on the three separate plots you made above, which alcgp-tobgp combination would you say carries the lowest risk for having esophageal cancer? What about highest risk? Make a plot with facets to see if your prediction is right.


ec %>% 
  ggplot(aes(x = agegp, y = ec_freq)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, color = "orange", alpha = 0.5) +
  facet_grid(alcgp ~ tobgp) +
  labs(title = "facet_grid") +
  theme_minimal()

ec %>% 
  ggplot(aes(x = agegp, y = ec_freq)) +
  geom_boxplot() +
  geom_jitter(width = 0.2, color = "blue", alpha = 0.5) +
  facet_wrap(alcgp ~ tobgp) +
  labs(title = "facet_wrap") +
  theme_minimal()
  

{r}
# using scatterplot
ec %>% 
  ggplot(aes(x = agegp, y = ec_freq)) +
  geom_point() +
  facet_grid(alcgp ~ tobgp) +
  labs(title = "facet_grid") +
  theme_minimal()

ec %>% 
  ggplot(aes(x = agegp, y = ec_freq)) +
  geom_point() +
  facet_wrap(alcgp ~ tobgp) +
  labs(title = "facet_wrap") +
  theme_minimal()

{r}
ec_summary <- ec %>%
  group_by(agegp, tobgp, alcgp) %>%
  summarise(mean_freq = mean(ec_freq), .groups = "drop") %>%
  arrange(desc(mean_freq))  # For highest risk

head(ec_summary, 1)    # Highest risk group
tail(ec_summary, 1)    # Lowest risk group


Next

So far we have barely scratched the surface of what ggplot2 can do. If you can dream it, you can probably create it with ggplot2.

At this point it is not possible nor meaningful to cover all the other plots you can make with ggplot2 because which plot you need depends entirely on the data you are analyzing and the questions you want to address. As you explore new data to address new questions, you might find that scatterplots and boxplots are no longer enough. That is to be expected. At that point, you can always learn what you need on the spot to create the plots that you want. It wouldn't take very long because you already understand conceptually how ggplot2 works.

This concludes the notebook on ggplot2. Now you are ready to apply everything you have learned so far to clean and explore a cancer cell line RNA-seq dataset! Proceed to "4_subset_ccle.qmd".
