##  Kata 01 - Supermarket Pricing
[Description](http://codekata.com/kata/kata01-supermarket-pricing/)

### Considerations

- We never keep fractional money, in order to avoid rounding number issues, keep dollars and cents as integers and separated.
- Prices are relatively fixed, we use an abstraction "PricingScheme" to change those prices in a less permanent way.
- Cost is how much you actually have to pay for something - So if something has a price of $1, but a 20% discount, the cost is $0.80
- PricingSchemes are responsible for applying discounts, and making them auditable (i.e. recording somewhere when a discount is used)

### Proposed Solution
```js
class Price { 
   constructor(price_as_float) { 
      this.dollars = Math.floor(price_as_float)
      this.cents = Math.floor(( price_as_float % 1 ) * 100) // This truncates after the second decimal.
                                                            // we might prefer rounding under some
                                                            // circumstances (I think truncate is better for the customer,
                                                            // rounding is better for the store)
   }

   add(other_price) { 
      this.cents += other_price.cents;
      carry = 0;

      if (this.cents > 100) {
        carry = 1;        
      }
      this.cents %= 100;

      this.dollars += other_price.dollars;
      this.dollars += carry;
   }
}

// A bill is the accumulation of items and the cost (not the price!).
// Prices are semi-static, but cost is flexible as it can have discounts applied to it based
// on what else is on the bill.
class Bill { 
    constructor() { 
        this.items = {} // Keep a dictionary of the items and quantities of each
        this.total = Price(0)
    }

    add_item(item_id) {
        // Retrieve the pricing scheme for that item
        let pricingScheme = // The scheme gotten in the previous step

        let result = pricingScheme.apply_on(this);
        
        if (result.discount) { 
           // record that a discount was applied
        }

        this.total.add(result.price);
    }
}

// A Pricing Scheme dictates how a Price is applied on an item.
class PricingScheme {

    constructor(item_id) { 
        this.item_id = item_id;
    }

    apply_on(bill) {
        // Get information about the item from some datastore or similar
        let price = item.price;
        let discount = null;
        return {price, discount}; 
    }
    
}

// For the Buy 3 get 1 free! cases - Create a general 'Buy N get K free' pricing scheme
// When an item is added to a bill - check the bill for other items with the same ID, and if N have been bought, the next
// K items will have a cost of $0 and will record the discount being applied

class BuyNGetK {
    constructor(item_id, buy_n, get_k) {
        super(item_id);
        this.buy_n = buy_n;
        this.get_k = get_k;
    }


}

```

```mermaid
```

