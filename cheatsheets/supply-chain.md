# Supply Chain Cheatsheet

Vocabulary for inventory, planning, systems, logistics, trade and forecasting, as used in supply chain customer work.

> Related: [Supply chain and AI questions](../topics/15-supply-chain-ai/README.md) · [Data and SQL questions](../topics/07-data-and-sql/README.md) · [Customer scenarios questions](../topics/09-customer-scenarios/README.md) · [ML fundamentals](ml-fundamentals.md) · [Data engineering](data-engineering.md) · [Billing and payments](billing-and-payments.md)

## Inventory

| Term | What it means |
|---|---|
| **Inventory** | Goods held as raw materials, work in progress or finished stock; protects service but ties up cash. |
| **SKU** (Stock Keeping Unit) | A unique identifier for a sellable or stocked item variant; the basic unit of planning and data. |
| **On-hand inventory** | Stock physically present at a location; not all of it may be available to promise. |
| **Safety stock** | Extra stock held to absorb demand and supply variability; raising service levels makes it grow non-linearly. |
| **Cycle stock** | Stock expected to be used between replenishment orders; driven by order size. |
| **Reorder point** | The stock level that triggers a new order: expected demand during lead time plus safety stock. |
| **Lead time** | Time from placing an order to having goods available; longer or more variable lead times mean more stock. |
| **Stockout** | Having no stock when demand occurs; loses sales and hides true demand in the sales data. |
| **EOQ** (Economic Order Quantity) | The order size that minimises ordering plus holding cost, √(2DS/H); assumes steady demand and fixed costs. |
| **MOQ** (Minimum Order Quantity) | The smallest order a supplier will accept; can force more stock than planning would otherwise want. |
| **Carrying cost** | The cost of holding stock, including capital, storage, insurance and obsolescence; often underestimated. |
| **ABC analysis** | Ranking items by revenue or value share, so the most important A items get the most attention. |
| **XYZ analysis** | Ranking items by demand variability; combined with ABC to set differentiated stock policies. |
| **FEFO** (First Expired, First Out) | Using stock with the earliest expiry first; essential for perishables and pharmaceuticals. |
| **FIFO** (First In, First Out) | Using the oldest stock first; reduces obsolescence and is also an accounting method. |
| **Cycle count** | Regularly counting a portion of stock rather than the whole warehouse; keeps system records accurate. |
| **Shrinkage** | Stock lost through theft, damage or errors; the gap between records and physical stock. |
| **Multi-echelon inventory** | Stock held across tiers such as plants, distribution centres and stores; optimising tiers separately overstocks the network. |
| **MEIO** (Multi-Echelon Inventory Optimisation) | Setting stock targets across all tiers together, usually lowering total stock for the same service. |
| **VMI** (Vendor-Managed Inventory) | The supplier monitors the customer's stock and decides replenishment; shares data and reduces bullwhip. |
| **Unit of measure** | The unit a quantity is recorded in, such as case or each; mismatches are a classic data quality problem. |

## Service and financial metrics

| Term | What it means |
|---|---|
| **OTIF** (On Time In Full) | Share of orders delivered complete and on the promised date; definitions vary, so always confirm them. |
| **Service level** | Target probability of not running out in a cycle; a higher level needs disproportionately more stock. |
| **Fill rate** | Share of demand met straight from stock, usually measured by units; differs from cycle service level. |
| **Inventory turns** | Annual cost of goods sold divided by average inventory; higher generally means stock moves faster. |
| **Days of supply** | How many days current stock covers at the expected demand rate; flags both excess and risk. |
| **DIO** (Days Inventory Outstanding) | Average days stock is held before selling; lower frees cash, but too low risks stockouts. |
| **DSO** (Days Sales Outstanding) | Average days to collect payment from customers; lower means cash returns sooner. |
| **DPO** (Days Payables Outstanding) | Average days taken to pay suppliers; higher keeps cash longer, but can strain supplier relationships. |
| **Cash-to-cash cycle** | DIO plus DSO minus DPO; how long cash is tied up between paying suppliers and being paid. |
| **Landed cost** | Total cost of getting goods to their destination, including purchase, freight, duty and insurance. |
| **Perfect order rate** | Share of orders that are on time, complete, undamaged and correctly documented; stricter than OTIF. |

### DIO vs DSO vs DPO

| Metric | Measures | Better for cash when |
|---|---|---|
| DIO | Time stock sits unsold | Lower |
| DSO | Time customers take to pay | Lower |
| DPO | Time taken to pay suppliers | Higher |

## Planning

| Term | What it means |
|---|---|
| **Demand forecasting** | Predicting future demand by item, location and time; valuable only when it changes a decision. |
| **S&OP** (Sales and Operations Planning) | A monthly cross-functional process agreeing one plan that balances demand and supply over the next 3 to 18 months. |
| **IBP** (Integrated Business Planning) | S&OP extended with financial plans and strategy, linking operations to business targets. |
| **Bullwhip effect** | Small demand changes amplifying into larger order swings further upstream; caused by batching, forecasting and shortage gaming. |
| **BOM** (Bill of Materials) | The structured list of components needed to make a product; multi-level BOMs drive material planning. |
| **MRP** (Material Requirements Planning) | Turns demand and BOMs into planned purchase and production orders; traditionally assumes infinite capacity. |
| **Push vs pull** | Push builds to a forecast; pull produces in response to actual demand. Most chains combine both. |
| **MTS** (Make to Stock) | Producing to forecast before orders arrive; fast delivery, but risks excess stock. |
| **MTO** (Make to Order) | Producing only after an order; low inventory, but longer delivery time. |
| **JIT** (Just in Time) | Receiving materials close to when they are needed; lowers stock but increases exposure to disruption. |
| **JIC** (Just in Case) | Holding extra stock as a buffer against disruption; more resilient but costlier. |
| **CPFR** (Collaborative Planning, Forecasting and Replenishment) | Trading partners share plans and forecasts to improve accuracy and reduce bullwhip. |

### Just in time vs just in case

| | JIT | JIC |
|---|---|---|
| Stock level | Low | High |
| Strength | Lower cost, less waste | Resilience to disruption |
| Weakness | Fragile when supply breaks | Ties up cash, obsolescence risk |

## Systems and data exchange

| Term | What it means |
|---|---|
| **ERP** (Enterprise Resource Planning) | The system of record for master data, orders and financial postings; SAP and Oracle are common examples. |
| **WMS** (Warehouse Management System) | Runs warehouse operations such as receiving, putaway, picking and packing, with location-level inventory. |
| **TMS** (Transport Management System) | Plans, executes and tracks shipments, including carrier selection, routing and freight costs. |
| **APS** (Advanced Planning and Scheduling) | Planning software that adds capacity and constraints beyond what basic MRP considers. |
| **MES** (Manufacturing Execution System) | Tracks and controls production on the factory floor in near real time. |
| **Master data** | Core reference records for materials, suppliers and customers; errors here spoil all downstream planning. |
| **EDI** (Electronic Data Interchange) | Standard B2B document exchange between trading partners; old but mandated and reliable, so integrations must handle it. |
| **PO** (Purchase Order) | A buyer's formal order to a supplier; a key document in the procure-to-pay flow (EDI 850). |
| **ASN** (Advance Ship Notice) | A supplier's electronic notice of what is shipping and how it is packed (EDI 856). |
| **Three-way match** | Checking purchase order, goods receipt and invoice agree before paying; catches errors and overbilling. |
| **Control tower** | A central view of supply chain status and exceptions across systems, supporting faster response to disruption. |

## Logistics and trade

| Term | What it means |
|---|---|
| **3PL** (Third-Party Logistics) | An outsourced provider of warehousing, transport or fulfilment; often a data integration point. |
| **4PL** (Fourth-Party Logistics) | A provider that manages the entire logistics network, including 3PLs, on the customer's behalf. |
| **DC** (Distribution Centre) | A facility that receives, stores and ships goods onward to stores or customers. |
| **Cross-docking** | Moving inbound goods directly to outbound transport with little or no storage; reduces handling and stock. |
| **FTL** (Full Truckload) | A shipment using a whole truck; faster and simpler for large volumes. |
| **LTL** (Less Than Truckload) | Sharing truck space with other shippers; cheaper for small loads, but slower with more handling. |
| **Last mile** | The final delivery leg to the end customer; typically the costliest per unit. |
| **Cold chain** | Temperature-controlled storage and transport for perishables and medicines; requires monitoring and audit trails. |
| **Expediting** | Paying more to speed up an order or shipment; costly, so track how often it happens. |
| **Vehicle routing problem** | Finding the cheapest delivery routes under constraints such as capacity and time windows; a classic optimisation problem. |
| **Tariff** | A government tax on imported goods; changes in rates can alter sourcing decisions *(verify)*. |
| **Incoterms** (International Commercial Terms) | Standard trade rules defining who pays for and bears risk at each shipping stage; updated periodically *(verify)*. |
| **EXW** (Ex Works) | The seller only makes goods available at its premises; the buyer handles nearly everything else. |
| **FCA** (Free Carrier) | The seller hands goods to the buyer's carrier at a named place; works for any transport mode. |
| **FOB** (Free On Board) | Risk passes once goods are on the vessel at the origin port; for sea and inland waterway only. |
| **CIF** (Cost, Insurance and Freight) | The seller pays freight and insurance to the destination port, but risk passes at loading; sea only. |
| **DAP** (Delivered at Place) | The seller delivers to the named place, with the buyer handling import duties. |
| **DDP** (Delivered Duty Paid) | The seller bears cost and risk through to destination, including import duties; maximum seller responsibility. |

### Incoterms at a glance

| Term | Risk passes to buyer | Import duty paid by |
|---|---|---|
| EXW | At seller's premises | Buyer |
| FCA | On handover to carrier | Buyer |
| FOB | On board at origin port | Buyer |
| CIF | On board at origin port | Buyer |
| DAP | At destination, ready to unload | Buyer |
| DDP | At destination, ready to unload | Seller |

## Forecasting and optimisation

| Term | What it means |
|---|---|
| **MAPE** (Mean Absolute Percentage Error) | Average percentage error per item; dangerous because it breaks at zero demand and overweights low-volume items. |
| **WAPE** (Weighted Absolute Percentage Error) | Total absolute error divided by total actual demand; more robust than MAPE for business reporting. |
| **Forecast bias** | Consistent over- or under-forecasting; often does more operational harm than random error. |
| **FVA** (Forecast Value Add) | Whether each step, such as a planner override, improves accuracy compared with a simple baseline. |
| **Backtesting** | Testing a forecast on past periods using only data available at the time; rolling origin is standard. |
| **ETS** (Error, Trend, Seasonality) | A family of classical exponential smoothing models; fast, explainable and often a strong baseline. |
| **ARIMA** (AutoRegressive Integrated Moving Average) | A classical statistical time-series model using past values and past errors. |
| **Intermittent demand** | Demand with many zero periods, typical of spare parts; standard forecasting and MAPE work badly on it. |
| **Censored demand** | True demand hidden because stockouts limited sales; treating sales as demand understates future needs. |
| **Cold-start forecast** | Forecasting a new product with no history, usually using similar products or launch attributes. |
| **Probabilistic forecast** (quantile forecast) | A forecast as a range of outcomes rather than one number; feeds safety stock and service decisions directly. |
| **Hierarchical reconciliation** | Making forecasts add up across levels such as SKU, category and region, so plans stay consistent. |
| **LP** (Linear Programming) | Optimising a linear objective under linear constraints; suits planning problems with continuous quantities. |
| **MIP** (Mixed-Integer Programming) | Linear programming with some whole-number decisions, such as open or close; more realistic but harder to solve. |
