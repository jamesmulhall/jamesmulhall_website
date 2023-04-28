---
title: "Forecasting diarrhoea incidence using weather data"
date: 2023-04-17T23:41:54-06:00
draft: false
featured_image: ![forecasting results](https://github.com/mullach/climate-sensitive-diseases/blob/main/Figures/pytorch_diarrhoea_pred.png?raw=true)
images: 
    - /images/deep_diarrhoea_predictions.png
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


## Over half a million children die from diarrhoea every year


Despite decreasing by 47% between 1990 and 2019, global deaths from diarrhoea remain bracingly high at 1.53 million annually. Half a million of these are deaths in children under 5 years old, representing the third highest cause of childhood mortality after lower respiratory diseases and neonatal disorders.[^2] In Vietnam, paediatric diarrhoea is associated with families with low income, an absence of piped water and toilets, malnutrition, household crowding, low maternal age, and poor education on sanitation.[^3] [^4] [^5] These factors are not equally weighted throughout the country—36% of households in the northern mountainous regions do not have access to clean water sources. Here, ethnic minority children under five are 3.5 times more likely to die of preventable causes.[^6] So, diarrhoea represents a significant and preventable cause of mortality that disproportionately affects ethnic minorities and low-income individuals. What can be done about it?


Oral rehydration solution is a cheap, effective treatment, and interventions promoting its use have had a significant impact on decreasing global mortality from diarrhoea.[^6] Other prevention measures include exclusive breastfeeding up to six months old, improved access to clean drinking water, improved sanitation education, and rotavirus vaccination.[^7] However, lack of access to treatment options remains a major issue — expanding the use of oral rehydration solution alone could prevent 30% of deaths from diarrhoea.[^8] Within Vietnam, the state-owned company POLYVAC has developed affordable rotavirus vaccines for more equitable access to immunisation.[^9] However, improved surveillance systems may be able to reduce cases of diarrhoeal diseases further.




#### What should we take away from this?


A few key conclusions are:
 1. Diarrhoea remains a significant cause of morbidity and mortality worldwide
 2. Effective treatment and prevention options exist and are underutilised
 3. More can be done to protect children from diarrhoea-related deaths




## How can modern technology help solve these issues?


Creating early-warning systems for outbreaks can help address these issues by exploiting correlations between weather patterns and diarrhoea incidence. For example, if heavy rainfall and flooding in Thái Bình are forecast to precede a spike in diarrhoea cases in one month, the national health service could focus efforts on implementing [WASH](https://www.unicef.org/wash) programs and distributing oral rehydration solution to the region.


These weather-diarrhoea relationships are not perfectly understood, but they have been studied. Researchers in Vietnam and further afield have found positive correlations between rainfall and diarrhoea incidence up to two months in advance.[^10] [^11] [^12] While not all studies have found the same results[^13], increased diarrhoea cases following high rainfall may be explained by high rainfall contaminating water sources with pathogens. This is a major issue in regions without clean water infrastructure, but rain-triggered floods can still overwhelm sewage systems and cause overflow into rivers and other bodies of water.[^14] Moreover, flooding can reduce access to medication and healthcare through damaged health centres, roads, and other transport infrastructure.[^15] Temperature and humidity have also been shown to correlate with diarrhoea incidence, but different studies have found positive[^10] [^12] [^16], negative[^13] [^17] [^18], or nonlinear[^19] results. Given that the relationships between weather patterns and diarrhoea incidence are only somewhat-understood, we set out to use deep learning models trained on each test province in Vietnam to forecast disease rates. This allowed us to account for non-linear relationships, interactions between weather variables, and variations in weather-diarrhoea relationships between different regions.


# Using weather patterns to forecast diarrhoea incidence


Our objective was straightforward — develop a range of machine learning models to forecast diarrhoea incidence in Vietnamese provinces up to 3 months ahead using weather data, and compare them to find the most accurate one. To set about doing this, we created a Seasonal AutoRegressive Integrated Moving Average (SARIMA) model, a multivariate SARIMA model (SARIMAX), a convolutional neural network (CNN), a Long Short Term Memory (LSTM) model, and an LSTM with an attention mechanism (LSTM-ATT). In our [paper on dengue fever](https://journals.plos.org/plosntds/article?id=10.1371/journal.pntd.0010509), we explored a broader range of simple machine-learning models. However, for this analysis, we kept only the SARIMA and SARIMAX models as, in general, the deep learning models (CNN, LSTM, and LSTM-ATT) outperformed them.


We selected the two best weather-variable predictors for each province using a random forest regressor[^20], and optimised hyperparameters with Optuna.[^21] We trained models using data from 1997–2010, validated them with data from 2011–2013, and tested them on data from 2014–2016. A full description of our methods can be found in my dissertation [here](/documents/mulhall_dissertation.pdf).


## Deep learning models accurately forecast diarrhoea rates in some, but not all, cases


Deep learning models accurately forecasted diarrhoea rates in some cases, but their overall and relative performance varied to a large extent between provinces. Models were able to predict general trends in diarrhoea incidence in most provinces, capturing big spikes and drops as seen in Cao Bằng and Thái Bình (Figure 1). However, none of the models would have been able to warn us of the major spike in diarrhoea cases in Lao Cai during month 5. It is difficult to say whether this failure is due to the models’ performance in general or weaker climate-diarrhoea associations in the province. Either way, it may be prudent to consider the usefulness of climate-based disease forecasting on a region-by-region basis. Before we dive deeper into the results, let’s review our error terms.




![forecasting results](/images/deep_diarrhoea_predictions.png)


{{< rawhtml >}}
    <p style="color: #808B96; text-align: center; font-size:90%">
        <i> Figure 1: Forecasting diarrhoea incidence one month ahead in five Vietnamese provinces. </i>
    </p>
{{< /rawhtml >}}




#### What do MAE, MAPE, and RMSE refer to?


- [**Mean Absolute Error (MAE)**](https://en.wikipedia.org/wiki/Mean_absolute_error): how wrong the predictions were, on average
    - A MAE of 10 would mean that the forecasts over- or under-estimated diarrhoea incidence rates by an average of 10 cases per 100,000 people
- [**Mean Absolute Percentage Error (MAPE)**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error): how wrong the predictions were, on average, as a percentage of the real diarrhoea incidence rates
    - A MAPE of 10% would mean that the forecasts over- or under-estimated diarrhoea incidence rates by an average of 10%.
- [**Root Mean Square Error (RMSE)**](https://en.wikipedia.org/wiki/Root-mean-square_deviation): how wrong the predictions were, on average, with larger errors having a disproportionately larger effect on the total score.
    - RMSE values are less tangible than MAE and MAPE, but they comparatively penalise models more if they are very wrong a few times than if they are slightly wrong many times.


Looking at our results a little more quantitatively supports our first impressions of the data — the models performed impressively well in some provinces but poorly in others. The best forecasts had MAPEs of 8–24% for four of the five provinces tested ([see: dissertation Table 5](/documents/mulhall_dissertation.pdf)). The true percentage errors in Điện Biên are obscured by an actual incidence rate of zero in the final test month, so mean absolute error (MAE) and root-mean-square error (RMSE) provide better evaluation metrics. An MAE of 32 for the best-performing model in the province (LSTM) is more informative, and suggests a half-decent-but-not-very-impressive performance when compared to the actual values hovering around 150–200 cases per 100,000 people in Figure 1. [link sentence]


There wasn't a clear winner, but LSTM-ATT and CNN models performed the best, on average (Figure 2). Looking at the average model rankings, LSTM-ATT came first using MAE-based scoring and CNN came first using RMSE-based scoring. It's worth noting the variation between provinces — every model other than SARIMAX came first in at least one province when considering both scoring systems. Therefore, it's likely a good idea to decide which model to use on a province-by-province basis, or to group the individual models into what's called a superensemble. However, for simplicity, we took LSTM-ATT as one of the best-performing models to later assess forecasts up to three months in advance.  


![forecasting results](/images/diarrhoea-1month-errors.png)


{{< rawhtml >}}
    <p style="color: #808B96; text-align: center; font-size:90%">
        <i> Figure 2: Performance of model performance across all five provinces in Vietnam using Root Mean Square Error (RMSE) and Mean Absolute Error (MAE). </i>
    </p>
{{< /rawhtml >}}


#### Long-Term Forecasts Are Usably Accurate


While forecasting errors worsened when forecasting further in advance, performance (arguably) remained high enough to be useful in some provinces. In Thái Bình, MAPE only increased from 8% to 11% when forecasting three months in advance. Similarly, in Cao Bằng, MAPE remained relatively low, increasing from 12% to 21%. Performance stayed essentially stable in Điện Biên, with RMSE and MAE actually decreasing when predicting further ahead. However, in Kon Tum and Lào Cai, the already high MAPEs rose to 31% and 42% — a point at which I would begin to question the value of these forecasts. While acknowledging this, I believe overall a 3-month early-warning system could still be beneficial. Long-term (2- and 3-month) forecasts could provide low-confidence scenarios to prepare for. This could allow low-cost preparations to be made, such as drafting up action plans, budgets, and necessary resources. If short-term (1-month) higher-accuracy forecasts later confirm the initial predictions, full outbreak response protocols could then be initiated.


![forecasting results](/images/diarrhoea-multi-month-errors.png)


{{< rawhtml >}}
    <p style="color: #808B96; text-align: center; font-size:90%">
        <i> Figure 3: Forecasting diarrhoea incidence k months ahead in five Vietnamese provinces using a Long Short-Term Memory model with an ATTention mechanism (LSTM-ATT). </i>
    </p>
{{< /rawhtml >}}


# What's the Bottom Line?


The models presented here have the potential to form early-warning systems for diarrhoea and other climate-sensitive infectious diseases, but require improvement. They would, of course, need to fit in as part of a holistic healthcare response; reducing the impact of diarrhoea will be impossible without crucial prevention and treatment measures such as oral rehydration solution, WASH programmes, clean water access, and rotavirus vaccination. Additional work to increase model accuracy across provinces would be essential for implementation throughout Vietnam. The next major step would be to test the ability of these early-warning models in real-time — are they able to prospectively forecast diarrhoea outbreaks three months from now? If so, they may form one puzzle piece in solving the pressing global issue of preventable deaths from diarrhoea.



### References

[^1]: https://www.npr.org/sections/goatsandsoda/2018/11/09/666150842/why-did-bill-gates-give-a-talk-with-a-jar-of-human-poop-by-his-side
[^2]: https://ourworldindata.org/childhood-diarrheal-diseases
[^3]: https://doi.org/10.1016/j.ijid.2015.03.013
[^4]: https://doi.org/10.1016/j.ijid.2005.05.009
[^5]: https://doi.org/10.1016/S1473-3099(18)30362-1
[^6]: https://www.unicef.org/vietnam/children-viet-nam
[^7]: https://www.who.int/news-room/fact-sheets/detail/diarrhoeal-disease
[^8]: https://www.thelancet.com/journals/laninf/article/PIIS1473-3099(18)30362-1/fulltext
[^9]: https://www.path.org/resources/using-innovation-combat-diarrheal-disease-vietnam/
[^10]: https://doi.org/10.1007/s00484-014-0942-1
[^11]: https://doi.org/10.1016/j.scitotenv.2016.12.027
[^12]: https://doi.org/10.1186/s12879-017-2611-6
[^13]: https://doi.org/10.1016/j.healthplace.2015.08.001
[^14]: https://doi.org/10.1371/journal.pone.0065112
[^15]: https://doi.org/10.3402%2Fgha.v4i0.6356
[^16]: https://doi.org/10.1371/journal.pone.0193246
[^17]: https://doi.org/10.1017/S0950268807008229
[^18]: https://doi.org/10.1017/S0950268815000709
[^19]: https://doi.org/10.1017/S0950268810002451
[^20]: https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html
[^21]: https://optuna.org/
