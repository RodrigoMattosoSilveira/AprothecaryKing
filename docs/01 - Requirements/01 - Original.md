# Hybrid Apothecary Operating Model

## 1. Business archetype

The apothecary is a **hybrid operation** combining three related but distinct business activities:

1. **Retail sales**
   The apothecary sells ready-made products directly to customers.

2. **In-house preparations**
   The apothecary produces small-batch or custom preparations using internal formulas, ingredients, packaging, and production workflows.

3. **Practitioner consultations**
   The apothecary provides consultations, recommendations, care notes, and follow-up guidance connected to customer needs and product use.

The software should treat these as separate operational lanes that share common records, especially:

* customers/clients
* products
* ingredients/materials
* inventory lots
* formulas
* batches
* orders
* labels
* staff/practitioners
* consultation records
* quality and traceability events

The key design principle is:

**Retail, preparation, and consultation should remain operationally distinct, but data-connected.**

## 2. Core operating boundary

The system should support a wellness-oriented hybrid apothecary unless the business later chooses to operate as a licensed pharmacy or regulated compounding pharmacy.

For now, the assumed boundary is:

* The business may sell retail wellness products.
* The business may prepare in-house products and custom blends.
* Practitioners may consult with clients and recommend products or preparations.
* The system should capture recommendations, instructions, cautions, and follow-up notes.
* The system should not assume prescription processing, insurance claims, controlled substances, or regulated pharmacy dispensing unless those are added later.

This boundary matters because the application should be operationally strong without prematurely becoming a pharmacy-management system.

## 3. Main operational lanes

## Retail lane

The retail lane covers products that are available for immediate sale.

Examples:

* bottled tinctures
* teas
* salves
* oils
* capsules
* soaps
* supplements
* packaged herbs
* wellness accessories
* third-party finished goods

Retail products may be:

* purchased from suppliers as finished goods
* produced internally in batches
* assembled as kits
* repackaged from bulk inventory, if allowed by business policy

Retail operations should support:

* product catalog
* shelf inventory
* barcode or SKU
* pricing
* tax category, if applicable
* available quantity
* lot and expiration tracking where appropriate
* reorder thresholds
* sales history
* returns
* recalls or product holds

Retail sales may or may not be connected to a known customer. The system should support both anonymous walk-in sales and customer-linked sales.

## In-house preparation lane

The preparation lane covers products made by the apothecary.

Examples:

* house tea blends
* custom tea blends
* tincture blends
* infused oils
* salves
* balms
* capsules
* powders
* syrups
* topical preparations
* practitioner-requested blends
* customer-specific preparations

Preparation operations should support:

* formulas/recipes
* formula versioning
* ingredient lot selection
* batch production
* custom preparation orders
* yield tracking
* waste tracking
* batch costing
* quality approval
* finished-goods inventory
* label generation
* expiration or best-by date assignment
* storage location assignment
* traceability from finished product back to ingredient lots

The system should distinguish between:

* **standard batch production** — made for general retail stock
* **custom preparation** — made for a specific customer or consultation
* **practitioner-directed preparation** — made based on a practitioner recommendation or care plan

## Practitioner consultation lane

The consultation lane covers the human-care side of the apothecary.

Consultations may include:

* intake notes
* customer goals or concerns
* allergies or sensitivities
* current products being used
* practitioner observations
* recommendations
* cautions
* product instructions
* follow-up tasks
* referral notes
* consent or acknowledgment, if needed

A consultation may result in:

* no sale
* retail product recommendation
* custom preparation request
* repeat/refill recommendation
* follow-up appointment
* practitioner note only

The system should connect consultations to downstream activity:

**consultation → recommendation → order/custom preparation → label/instructions → follow-up**

The system should also preserve the difference between:

* general customer notes
* practitioner notes
* preparation instructions
* label instructions
* internal staff-only notes
* follow-up reminders

## 4. Shared operational backbone

The hybrid apothecary should be modeled around a shared operational backbone:

**supplier → purchase order → receiving → inventory lot → storage → formula → batch/preparation → product/order → customer → follow-up**

This means the software should not treat inventory, consultations, and sales as disconnected modules.

For example:

A practitioner recommends a custom tea blend.
The system creates a custom preparation order.
The preparation order reserves ingredients.
The ingredients are selected from approved inventory lots.
The preparation is made, labeled, and stored.
The customer picks it up.
The consultation record shows what was recommended and when follow-up is due.
If an ingredient lot is later recalled, the system can identify the customer who received the blend.

That is the value of the hybrid model.

## 5. Refined core entities

The following entities become especially important for this archetype.

## Customer / Client

Represents a person who buys products, receives recommendations, requests custom preparations, or attends consultations.

Key data:

* name
* contact information
* communication preferences
* purchase history
* consultation history
* known sensitivities or cautions
* follow-up reminders
* active custom orders
* notes

## Practitioner

Represents a staff member or associated professional who performs consultations or makes recommendations.

Key data:

* name
* role
* credentials, if applicable
* active/inactive status
* consultation history
* recommendation history

## Consultation

Represents an interaction between a practitioner and a client.

Key data:

* client
* practitioner
* date/time
* reason for visit
* intake notes
* recommendation notes
* cautions
* recommended products
* requested custom preparations
* follow-up date
* internal notes

## Recommendation

Represents a structured recommendation made during or after a consultation.

A recommendation may point to:

* a retail product
* a formula
* a custom preparation
* a lifestyle or usage instruction
* a follow-up action

## Product

Represents a sellable item.

Product types may include:

* retail finished good
* internally produced finished good
* custom preparation
* service/consultation
* kit or bundle
* packaging/resale item

## Formula

Represents a reusable recipe or preparation definition.

Key data:

* formula name
* version
* intended product type
* ingredients/materials
* preparation steps
* expected yield
* default label instructions
* default cautions
* shelf-life rule
* status: draft, active, retired

## Batch

Represents a production run.

Key data:

* formula
* batch number
* produced date
* produced by
* ingredient lots consumed
* quantity produced
* waste/loss
* finished product lot
* quality status
* storage location
* approval status

## Custom Preparation Order

Represents a customer-specific preparation request.

Key data:

* customer
* consultation reference, if applicable
* practitioner reference, if applicable
* formula or custom formula
* requested quantity
* preparation notes
* label instructions
* status
* due date
* assigned preparer
* inventory reserved
* completed batch/preparation reference

## Supplier

Represents a source of ingredients, packaging, merchandise, or finished goods.

## Purchase Order

Represents an order placed with a supplier.

## Receiving Record

Represents goods received into the business.

## Inventory Lot

Represents a traceable quantity of material or finished goods.

## Storage Location

Represents where inventory physically resides.

## Order / Sale

Represents the commercial transaction.

An order may include:

* retail products
* custom preparations
* internally produced products
* consultation services
* shipping or pickup
* discounts
* payment status
* fulfillment status

## Label / Instructions

Represents customer-facing usage, storage, caution, and identity information for a product or preparation.

## Quality Event

Represents a complaint, recall, adverse reaction, quarantine, rejected material, supplier issue, or corrective action.

## Audit Event

Represents system accountability for significant changes.

## 6. Major workflows

## Retail sale workflow

**customer selects product → staff scans/adds product → system checks availability → sale completed → inventory decremented → receipt issued**

Optional customer-linked version:

**customer identified → sale completed → purchase history updated → follow-up suggested if appropriate**

## Standard in-house batch workflow

**formula selected → batch planned → ingredient lots selected → inventory consumed → product produced → quality approved → finished goods stored → retail inventory increased → labels generated**

## Custom preparation workflow

**client request or consultation → recommendation → custom preparation order → inventory reserved → preparation made → label generated → ready for pickup/shipping → completed → follow-up scheduled**

## Practitioner consultation workflow

**client intake → practitioner notes → recommendations → products or custom preparations selected → instructions generated → follow-up scheduled**

## Purchase and receiving workflow

**low stock or production need identified → purchase order created → supplier confirms → merchandise received → quantities and lots recorded → goods inspected → inventory created → storage location assigned**

## Inventory movement workflow

**inventory received → stored → moved → reserved → consumed/sold → adjusted/disposed if needed**

## Traceability workflow

Forward trace:

**supplier lot → receiving record → inventory lot → batch → finished product lot → customer orders**

Reverse trace:

**customer complaint → order → product lot → batch → ingredient lots → supplier and purchase order**

## 7. Key management challenges for the hybrid model

The hybrid model has more complexity than ordinary retail because the business combines **commerce, production, and advisory care**.

The biggest challenges are:

## 1. Keeping retail, preparation, and consultation connected

A generic POS may track sales but not consultations.
A generic CRM may track customers but not inventory lots.
A generic inventory system may track stock but not formulas or batches.

The hybrid apothecary needs all three connected.

## 2. Avoiding reliance on expert memory

Practitioners and experienced staff often remember formulas, customer preferences, substitutions, supplier quality, and preparation habits. The system should preserve that knowledge in structured records.

## 3. Managing lot-aware inventory

The business needs to know not only what is in stock, but:

* what lot it came from
* when it expires
* where it is stored
* whether it is approved for use
* whether it is reserved
* whether it has been used in a preparation
* which customers ultimately received it

## 4. Managing custom work

Custom preparations need clear status tracking. Without software support, staff may lose track of what needs to be made, who requested it, when it is due, what instructions apply, or whether ingredients are available.

## 5. Maintaining consistency in practitioner recommendations

Practitioner consultations should remain personal, but recommendations should be recorded clearly enough that staff can prepare, label, fulfill, and follow up correctly.

## 6. Pricing correctly

The system should help price products and preparations using:

* ingredient cost
* packaging cost
* labor
* waste/loss
* consultation time
* overhead
* desired margin

## 7. Controlling expiration and waste

The system should help staff use older approved inventory first, identify expiring inventory, avoid overproduction, and prevent expired products from being sold or used.

## 8. Supporting traceability and recalls

The business must be able to quickly answer:

Who received this product?
Which batch did it come from?
Which ingredients were used?
Which supplier provided them?
What else was made from the same lot?

## 8. Best software opportunities for this archetype

The best software opportunity is to create a **hybrid apothecary operations platform**.

The strongest modules are:

1. **Customer and consultation management**
2. **Recommendation management**
3. **Product catalog**
4. **Formula and batch management**
5. **Custom preparation workflow**
6. **Purchase order management**
7. **Receiving and lot capture**
8. **Inventory storage and movement**
9. **Label generation**
10. **Retail/order fulfillment**
11. **Traceability and quality events**
12. **Management dashboard**

## 9. MVP scope for the hybrid model

The MVP should focus on the operational spine rather than trying to become a full POS, e-commerce system, or pharmacy system immediately.

## MVP core records

* customers/clients
* practitioners/staff
* suppliers
* products
* ingredients/materials
* formulas
* purchase orders
* receiving records
* inventory lots
* storage locations
* consultations
* recommendations
* custom preparation orders
* batches
* orders/sales
* labels

## MVP workflows

* create customer
* record consultation
* create recommendation
* create retail product
* create ingredient/material
* create formula
* create purchase order
* receive merchandise
* create inventory lot
* assign inventory to storage
* move inventory
* create batch from formula
* consume ingredient lots
* produce finished product lot
* create custom preparation order
* generate label
* fulfill order
* trace finished product back to ingredient lots

## MVP management views

* open consultations requiring follow-up
* open custom preparation orders
* low stock
* open purchase orders
* late purchase orders
* partially received purchase orders
* received items pending approval
* inventory by lot
* inventory by location
* expiring soon
* pending batches
* products ready for sale
* batch traceability
* customer purchase and recommendation history

## 10. Guiding product principle

The application should help the apothecary answer five operational questions at any time:

1. **Who are we helping?**
2. **What did we recommend or sell?**
3. **What did we make it from?**
4. **Where is it now?**
5. **Can we trace it if something goes wrong?**

That should be the foundation of the requirements model.
