# Association Rules — Market Basket Analysis

A collection of market basket analysis implementations using two classic association rule mining algorithms: **Apriori** and **ECLAT**. Both are applied to a real-world grocery transactions dataset to uncover hidden purchasing patterns and product relationships.


---

## Algorithms

### Apriori

The Apriori algorithm is a classic association rule learning method that uses a **bottom-up, breadth-first** search strategy over the itemset lattice. It generates frequent itemsets by iteratively pruning candidates that don't meet a minimum support threshold, then derives association rules from those frequent itemsets.

Three metrics are used to evaluate rules:

$$\text{Support}(A \Rightarrow B) = \frac{\text{Transactions containing } A \cup B}{\text{Total transactions}}$$

$$\text{Confidence}(A \Rightarrow B) = \frac{\text{Support}(A \cup B)}{\text{Support}(A)}$$

$$\text{Lift}(A \Rightarrow B) = \frac{\text{Confidence}(A \Rightarrow B)}{\text{Support}(B)}$$

A **Lift > 1** indicates a positive association between items — they appear together more often than by chance.

**Parameters used:**

| Parameter | Run 1 | Run 2 |
|-----------|-------|-------|
| `min_support` | 0.003 | 0.004 |
| `min_confidence` | 0.2 | 0.2 |
| `min_lift` | 3 | 3 |
| `min_length` | 2 | — |
| `max_length` | 2 | — |

**Top Association Rules (sorted by Lift):**

| Left Hand Product | Right Hand Product | Support | Confidence | Lift |
|---|---|---|---|---|
| fromage blanc | honey | 0.003333 | 0.245098 | 5.164 |
| light cream | chicken | 0.004533 | 0.290598 | 4.844 |
| pasta | escalope | 0.005866 | 0.372881 | 4.701 |
| pasta | shrimp | 0.005066 | 0.322034 | 4.507 |
| whole wheat pasta | olive oil | 0.007999 | 0.271493 | 4.122 |
| herb & pepper | ground beef | 0.015998 | 0.323450 | 3.292 |
| tomato sauce | ground beef | 0.005333 | 0.377358 | 3.841 |
| mushroom cream sauce | escalope | 0.005733 | 0.300699 | 3.791 |

---

### ECLAT (Equivalence Class Transformation)

ECLAT is a more efficient association rule algorithm that uses a **depth-first search** strategy with a vertical data representation (tid-lists). Unlike Apriori, ECLAT focuses solely on **Support** to rank item pair relationships, making it computationally faster for large datasets.

$$\text{Support}(A, B) = \frac{|tid\text{-}list(A) \cap tid\text{-}list(B)|}{\text{Total transactions}}$$

**Parameters used:**
- `min_support` = 0.003
- `min_confidence` = 0.2
- `min_lift` = 3
- Pair-length constrained (min_length=2, max_length=2)

**Top Association Rules (sorted by Support):**

| Left Hand Product | Right Hand Product | Support |
|---|---|---|
| herb & pepper | ground beef | 0.015998 |
| whole wheat pasta | olive oil | 0.007999 |
| pasta | escalope | 0.005866 |
| mushroom cream sauce | escalope | 0.005733 |
| tomato sauce | ground beef | 0.005333 |
| pasta | shrimp | 0.005066 |
| light cream | chicken | 0.004533 |
| fromage blanc | honey | 0.003333 |
| light cream | olive oil | 0.003200 |

**Key Difference from Apriori:** ECLAT's results DataFrame contains only `[Left Hand Product, Right Hand Product, Support]` — no Confidence or Lift — reflecting the algorithm's focus on frequency of co-occurrence rather than directional rule strength.

---

## Dataset

**`Market_Basket_Optimisation.csv`**
- 7,500 customer transactions from a French superstore
- Each row represents one shopping transaction
- Up to 20 products per transaction
- No column headers — products stored as raw string values
- Covers ~120 unique grocery items (e.g., mineral water, eggs, spaghetti, chocolate, french fries)

**Preprocessing:**
```python
dataset = pd.read_csv('Market_Basket_Optimisation.csv', header=None)
transactions = []
for i in range(0, dataset.shape[0]):
    transactions.append([str(dataset.values[i, j]) for j in range(0, dataset.shape[1])
                         if not pd.isna(dataset.values[i, j])])
```

## Tech Stack

• Language -	Python 3.10.9
• Algorithm Library -	apyori 1.1.2
• Data Handling -	pandas, numpy
• Visualization -	matplotlib
• Environment -	Jupyter Notebook (Anaconda)

## Getting Started

1. Clone the repository
```bash
git clone https://github.com/nithinrajkore/Association-Rules.git
cd Association-Rules
```

2. Install dependencies
```bash
pip install apyori pandas numpy matplotlib
```

3. Run the notebooks

• Navigate to Apriori/Apriori.ipynb for full Apriori analysis (support + confidence + lift)

• Navigate to Eclat/Eclat.ipynb for ECLAT analysis (support-focused)

---

## Key Concepts

Itemset - 	A set of one or more products

Frequent Itemset -	An itemset whose support meets the minimum threshold

Association Rule -	An implication of the form A → B

Support -	How often items appear together in all transactions

Confidence -	How often rule A → B is correct when A occurs

Lift -	How much more likely B is purchased when A is purchased, compared to random chance

## Results Interpretation

• Lift > 1: Positive association — items are purchased together more often than expected

• Lift = 1: Items are independent of each other

• Lift < 1: Negative association — items are purchased together less often than expected

The strongest discovered rule is fromage blanc → honey (Lift: 5.16), meaning customers who buy fromage blanc are over 5× more likely to also buy honey compared to the average customer.



