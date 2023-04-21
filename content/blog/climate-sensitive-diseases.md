---
title: "Forecasting diarrhoea incidence using weather data"
date: 2023-04-17T23:41:54-06:00
draft: true
featured_image: ![forecasting results](https://github.com/mullach/climate-sensitive-diseases/blob/main/Figures/pytorch_diarrhoea_pred.png?raw=true)
---

<!-- *James Mulhall* &emsp; | &emsp; *April 18th 2023* -->
<!-- *James Mulhall*  -->
<!-- \ -->
<!-- *April 18th 2023* -->

{{< rawhtml >}}
    <style>
        .content {
            margin-top: -3rem}
    </style>
{{< /rawhtml >}}

![Diarrhoea rates map](/images/childhood_deaths_causes.png)

<!-- {{< rawhtml >}}
    <p style="color: #808B96; text-align: center; font-size:90%">
        <i> Figure 1: Childhood deaths from the five most lethal infectious
            diseases worldwide, World, 2019 </i>
    </p>
{{< /rawhtml >}} -->

<!-- ![Diarrhoea rates by province in Vietnam](https://github.com/mullach/climate-sensitive-diseases/blob/main/Figures/diarrhoea_rates_by_province.png?raw=true) -->

# Introduction

> "‘To have any hope of solving sanitation problems, we have to break taboos and get over our discomfort in talking about poop." — Jim Yong Kim, 12th President of the World Bank.[^1]

Early warning systems for infectious diseases have the potential to flag outbreaks to public health authorities months in advance. This can help facilitate a targeted response to epidemic mitigation, directing healthcare resources to regions within a country that need them most. I took part in a research collaboration between universities and public health organisations in Northern Ireland and Vietnam, where we developed forecasting models to help improve public health responses to dengue fever and diarrhoea in Vietnam. This article focuses on diarrhoea, as our work on dengue fever has already been published [here](https://journals.plos.org/plosntds/article?id=10.1371/journal.pntd.0010509). It also focuses on diarrhoea in an attempt to move past stigma — though perhaps not quite as strong an attempt as [Bill Gates bringing a jar of poop on stage](https://www.npr.org/sections/goatsandsoda/2018/11/09/666150842/why-did-bill-gates-give-a-talk-with-a-jar-of-human-poop-by-his-side).

#### Over half a million children die from diarrhoea every year

Despite decreasing by 47% between 1990 and 2019, global deaths from diarrhoea remain bracingly high at 1.53 million annually. Half a million of these are deaths in children under 5 years old, representing the third highest cause of childhood mortality after lower respiratory diseases and neonatal disorders.[^2] In Vietnam, paediatric diarrhoea is associated with families with low-income, an absence of piped water and toilets, malnutrition, household crowding, low maternal age, and poor education on sanitation (Anders et al., 2015; Nguyen et al., 2006, Troeger et al., 2018). These factors are not equally weighted throughout the country—36% of households in the northern mountainous regions do not have access to clean water sources. Here, ethnic minority children under five are 3.5 times more likely to die of preventable causes (UNICEF, 2018). 

Oral rehydration solution is a cheap, effective treatment for diarrhoea, and interventions promoting its use have had a significant impact on decreasing global mortality from diarrhoea (Troeger et al., 2018). Other prevention measures include exclusive breastfeeding up to six months old, improved access to clean drinking water, improved sanitation education, and rotavirus vaccination (World Health Organisation, 2017). However, lack of access remains a major issue — expanding the use of oral rehydration solution alone could prevent 30% of deaths from diarrhoea (https://www.thelancet.com/journals/laninf/article/PIIS1473-3099(18)30362-1/fulltext). 

To address limited rotavirus uptake due to high cost and lack of availability, the Vietnamese state-owned company POLYVAC has developed affordable rotavirus vaccines for more equitable access (PATH, 2019).


#### What should we take away from this?

A few key conclusions are that:
 1. Diarrhoea remains a significant cause of morbidity and mortality worldwide
 2. Effective treatment and prevention options exist and are underutilised
 3. More can be done to protect children from diarrhoea-related deaths


## Using weather patterns to forecast diarrhoea incidence rates

Creating early-warning systems for diarrhoea outbreaks can help address these issues. For example, if heavy rainfall and flooding in the Vietnamese region of Thái Bình is forecast to cause a spike in diarrhoea cases in one month’s time, the national health service could focus efforts on WASH programs distributing oral rehydration solution to the region. 

MENTION SOMETHING ABOUT WEATHER-SENSITIVITY!!!

Our objective was straightforward — develop a range of machine learning models to forecast diarrhoea incidence in Vietnamese provinces up to 3 months ahead using weather data, and compare them to find the most accurate one. To set about doing this, we created a Seasonal AutoRegressive Integrated Moving Average (SARIMA) model, a multivariate SARIMA model (SARIMAX), a convolutional neural network (CNN), a Long Short Term Memory (LSTM) model, and an LSTM with an attention mechanism (LSTM-ATT). In our paper on dengue fever (LINK), we explored a broader range of simple machine learning models. However, for this analysis we kept only the SARIMA and SARIMAX models as, in general, the deep learning models (CNN, LSTM, and LSTM-ATT) outperformed them.

We selected two weather variable for each province using a random forest regressor (https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html), and optimised hyperparameters with Optuna (https://optuna.org/). We trained models using data from 1997–2010, validated them with data from 2011–2013, and tested them on data from 2014–2016. A full description of our methods can be found in my dissertation, here.

![forecasting results](/images/deep_diarrhoea_predictions.png)

blah blah blah

![forecasting results](/images/diarrhoea-multi-month-errors.png)

blah blah blah

![forecasting results](/images/df_model_rankings_cropped.png)


### References

[^1]: https://www.npr.org/sections/goatsandsoda/2018/11/09/666150842/why-did-bill-gates-give-a-talk-with-a-jar-of-human-poop-by-his-side
[^2]: https://ourworldindata.org/childhood-diarrheal-diseases