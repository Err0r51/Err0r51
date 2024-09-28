---
title: On data-driven venture capital
date: 2024-09-28 17:41:05
tags:
---
It has been a while since I wrote my last post. I have been busy with my master thesis - which I finally submitted. Below I have summarized my research on data-driven venture capital with the title **"Gut versus Data: Evidence from data-driven VC"**. I hope you enjoy it.


## Motivation for Research
The venture capital (VC) industry is a critical driver of innovation, providing essential funding to early-stage startups with high growth potential. Traditional methods of evaluating startups, which rely heavily on human judgment and personal networks, have inherent limitations such as biases, inefficiencies, and a lack of transparency. With the advent of data-driven approaches, there is a significant opportunity to enhance the screening and selection processes in VC, potentially leading to more accurate and efficient investment decisions. This research explores a novel hybrid approach that integrates automated scorecards with Large Language Models (LLMs) to improve the decision-making process in venture capital.

## Existing Knowledge
Previous research has established the importance of various factors in VC decision-making, including the experience of the founding team, the potential market size, and the innovativeness of the product. Data-driven methods, including machine learning models, have shown potential in predicting successful outcomes but are often constrained by the quality and structure of the available data. Traditional scorecard approaches are commonly used to standardize evaluations, but they can oversimplify the complexity involved in investment decisions.

## Research Gaps
Despite the advancements in data-driven methodologies, the integration of these models into the VC screening process, particularly in a way that maintains transparency and reduces biases, remains underexplored. The efficacy of LLMs in processing unstructured data to provide meaningful insights that align with human decision-making is not yet fully understood. Additionally, the performance of such hybrid models in real-world VC environments, where data may be incomplete or inconsistent, requires further investigation.

## Research Question
This research addresses the following question: Can LLM-augmented scorecards enhance the accuracy and efficiency of the screening process in venture capital, and how do these models compare to human analysts in identifying successful investment opportunities?

## Theoretical Framework
This research is grounded in decision support systems and the application of explainable artificial intelligence (XAI) in investment decision-making. The study builds on existing models that utilize scorecards and machine learning in venture capital, introducing LLMs to enhance the processing of unstructured data. The primary hypothesis is that LLM-augmented scorecards can improve the accuracy of predicting successful investment outcomes compared to traditional human-driven approaches.

## Empirical Approach
The research employs an empirical approach, analyzing historical data from a European early-stage VC spanning six years (2017-2023). The dataset comprises over 7,400 companies and 9,500 founders, with a focus on evaluating the model's performance in predicting successful outcomes, such as follow-up funding rounds, acquisitions, or IPOs. The model integrates both qualitative and quantitative features related to the founders, products, and market dynamics. The LLM-augmented scorecard's effectiveness is then compared to that of human analysts.

## Research Results
The LLM-augmented scorecard model demonstrated a 72% accuracy (AUC) in predicting successful outcomes across six cross-validation folds. This marks a significant improvement over traditional scorecard methods. However, human analysts outperformed the model in selecting companies that secured successful outcomes, particularly in raising substantial follow-up funding from top-ranked VCs or reaching an IPO. The model effectively processed unstructured data and reduced certain biases but struggled to predict outcomes for companies that were acquired or went public. In terms of funding predictions, the model showed that for each unit increase in its score, there was an associated increase of approximately $9,000 in the total funding raised by the company. This finding underscores the model’s utility in estimating the financial potential of startups, providing VCs with a data-driven method to prioritize companies likely to secure substantial investment.

However, the model's performance in predicting investor rankings was less impressive. An ordinal logistic regression revealed that the model’s scores had little to no impact on predicting whether a company would attract top-tier investors, highlighting a significant area for improvement.

When comparing the model’s performance to that of human analysts, the results were mixed. Although the model identified 196 successful companies compared to 68 identified by the human analysts, the logistic regression analysis indicated that human selection was a more accurate predictor of success, with a coefficient of 2.9304 compared to the model’s 0.2261. This suggests that while the model is effective at filtering out weaker opportunities and identifying potential winners, it lacks the nuanced judgment that human analysts bring to the decision-making process.


## Discussion of Results
The findings suggest that while LLM-augmented scorecards offer a valuable tool for enhancing the screening process in venture capital, they are not yet capable of fully replacing human decision-making. The model’s transparency and ability to process unstructured data provide meaningful insights; however, its limitations in predicting certain outcomes highlight the need for further refinement. The weak correlation between model scores and human decisions indicates that there are unmodeled factors influencing human decision-making that the current model does not capture.

## Implications for Research and Practice
For research, this study highlights both the potential and the limitations of integrating LLMs into decision support systems within the venture capital industry. It paves the way for further exploration of hybrid models that combine human intuition with data-driven insights. For practitioners, the findings suggest that VCs can benefit from using LLM-augmented scorecards to streamline the screening process with an augmented approach, but human oversight remains essential for making end-to-end investment decisions.

## Conclusion
This research contributes to the ongoing development of data-driven tools in the VC sector by introducing and testing a novel LLM-augmented scorecard model. While the model shows promise in improving the efficiency and accuracy of the screening process, it is not yet capable of fully replacing human analysts. The integration of such tools can enhance decision-making processes, but a balance between automation and human judgment is crucial for achieving optimal investment outcomes.

# Key References
- Arroyo, V., Concha, G., & Lecuona, P. (2019). "Assessment of Machine Learning Models in Venture Capital Decision-Making." Journal of Business Research, 102, 275-287.
- Bai, S., & Zhao, Y. (2021). Startup investment decision support: Application of venture capital scorecards using machine learning approaches. Systems, 9(3), 55.
- Gompers, P., & Lerner, J. (2019). "How Venture Capitalists Make Decisions." Harvard Business Review, 97(2), 88-97.
- Kaplan, S. N., & Strömberg, P. (2004). "Characteristics, Contracts, and Actions: Evidence from Venture Capitalist Analyses." The Journal of Finance, 59(5), 2173-2206.
- Lyonnet, V., & Stern, N. (2022). "Venture Capital Misjudgment and the Limits of Algorithmic Screening." Venture Capital Journal, 28(1), 123-141.
- Retterath, A. (2020). "Benchmarking Venture Capitalists and Machine Learning Algorithms for Investment Screening." Journal of Financial Economics, 137(2), 368-391.
- Shepherd, D. A., & Zacharakis, A. (2010). "Conjoint Analysis in Venture Capital Research." Venture Capital, 12(3), 173-194.
- Weibl, M., & Eckhardt, J. T. (2019). "Finding the Next Unicorn: The Evolution of Venture Capital Deal Origination." Strategic Management Journal, 40(2), 313-335.
- Nie, W., & Han, J. (2024). "A Survey on Large Language Models in Finance." IEEE Transactions on Neural Networks and Learning Systems, 35(1), 32-50.
