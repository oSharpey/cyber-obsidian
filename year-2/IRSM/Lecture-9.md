In brief, calculating a metric for the impact and likelihood of an event happening

TODO: slide 4
## Aggregation 

For small/medium businesses RM can be overwhelming, aggregation can be used to mitigate this.

This is the grouping of assets, threats and their risks into more general categoires.

# Types of Analysis Method

- Quantitative
	- mathematical approach to the problem. 
	- measure the amount of damage done to an asset because of a compromise
	- identifies probabilities
- Qualitative
	- estimated potential loss is used
	- “high”, “medium”, and “low”
	- involves less uncertainty 
	- considers the knowledge and the judgements of those doing the analysis. 
- Knowledge-Based
	- reusing “best practice” from similar systems
- Model-Based
	- object-oriented modelling to describe and analyse the risk

![[anal_methods.png]]
## Quantitative Example

![[quant_example.png]]


# Risk Metrics

$risk = (likelihood * impact) \pm uncertainty$

### Uncertainty

TODO ask what this is with respect to

An estimate made with respect to controls
The degree to which a current control can reduce risk
### Impact
what?
### Likelihood
Depends on the value of information and motivation of the attacker

Attacker will be present in some form, except for environmental hazards, errors and failures
![[likelihood_est.png]]
## Example
![[risk_calc_example.png]]

# Risk Heatmap

![[rating_key.png]]
![[heatmap.png]]

# Deliverables
![[ra_deliverables.png]]


# Risk evaluation and treatment
Each organization must determine its risk appetite during risk evaluation.

Assets with unacceptable levels of risk are treated using one of five strategies:
- **Defence**: Preventing the exploitation of a vulnerability
- **Transference**: Shifting risk to another entity 
- **Mitigation**: Planning and prep to reduce the impact or potential consequences of an incident
- **Acceptance**: Do nothing beyond current protections
- **Termination**: Organisations intentional choice not to protect an asset

## Mitigation Plan Example
![[mit_plan_example.png]]


## Managing the risk
- Risk appetite is the quantity and nature of risk that organisations are willing to accept as they evaluate the trade-offs between perfect security and unlimited accessibility
	- A reasoned approach balances the expense against the possible losses, if exploited
	- When vulnerabilities have been controlled to the degree possible, there is often remaining risk that has not been completely removed, shifted, or planned for— residual risk

### Residual Risk
- Often there is some remaining risk that has not been completely removed, shifted, or planned for
- Residual risk persists even after safeguards are implemented

![[residual_risk.png]]

### Risk Handling
- The goal is to bring the risk in line with an organisation’s risk appetite, not zero
- If decision makers and the authority groups decide to leave residual risk in place, then the information security program has accomplished its primary goal
![[risk_handling.png]]

### Risk Treatment
- **A vulnerability exists in an important asset**: Implement security controls to reduce the likelihood of a vulnerability being exploited
- **A vulnerability can be exploited**: Apply layered protections, architectural designs, and administrative controls to minimize the risk or prevent the occurrence of an attack
- **An attacker’s potential gain is greater than the costs of attack**: Apply protections to increase the attacker’s cost or reduce the attacker’s gain by using technical or managerial controls
- **A potential loss is substantial**: Apply design principles, architectural designs, and technical and nontechnical protections to limit the extent of the attack, thereby reducing the potential for loss

*Each information threat/vulnerability/asset (TVA) triplet that was developed in the risk assessment should have a documented treatment strategy that clearly identifies any residual risk that remains after the proposed strategy has been executed*

![[risk_treatment.png]]


# Cost Benefit Analysis 

| Cost | Benefit |
| ---- | ------- |
| - difficult to determine <br> - Items that affect the cost of a control or safeguard include: <br> - Cost of development or acquisition <br>- Training fees  <br> - Cost of implementation <br> - Service costs <br> - Cost of maintenance  <br> - Potential cost from the loss of the asset  | - the value to the organisation of using controls to prevent losses associated with a specific vulnerability <br> - determined by valuing the information asset or assets exposed by the vulnerability and then determining how much of that value is at risk and how much risk there is for the asset <br> - Annualised loss expectancy (ALE) |

Cost avoidance is the money saved by using the defence strategy via the implementation of a control, thus eliminating the financial ramifications of an incident
- The criterion most used when evaluating a strategy to implement controls and safeguards is economic feasibility

## Other Methods of Feasibility
- Organisaional Feasibility
- Operational Feasibility
- Technical Feasibility
- Political Feasibility

# Alternative Risk Treatment Practices

## (Semi) Qualitative and Hybrid Asset Valuation Measures
- Execute risk assessment steps using estimates based on a qualitative assessment
- More granular approach
	- tries to reduce some of the ambiguity of qualitative measures without resorting to the unsubstantiated estimations used for quantitative measures


## Delphi Technique
- A process whereby a group rates or ranks a set of information
	- The individual responses are compiled and then returned to the group for another iteration
	- This process continues until the entire group is satisfied with the result
	- This technique can be applied to the development of scales, asset valuation, asset or threat ranking, or any scenario that can benefit from the input of more than one decision maker


# Asset Valuation

## Single loss expectancy
- Estimating the likelihood of an attack based on an annualised rate of occurrence and the impact of an attack based on loss expectancy
- Considers the value of the asset and the expected percentage of loss that would occur from a particular attack
- **Single loss expectancy (SLE) = the calculated value associated with the most likely loss from a single occurrence of a specific attack
• Exposure factor (EF) = the percentage loss that would occur from a given
vulnerability being exploited – usually estimated