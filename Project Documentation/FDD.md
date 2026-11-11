# FDD Little shop Game
## General description
Little Shop is a cozy 2D organization game where the player is a employee worker. 

The player's job is to keep the store clean organized, restock products, arrange shelves, and help customers find what they need.

The shop changes throughout the year with different seasons. Each season introduces new products, decorations, and items that customers can purchase. During winter the shop sells hot chocolate, winter decorations, and other seasonal snacks, but in summer their will be ice cream, drinks and activity items.

## Dynamics
### The main gameplay loop is:
- Start the workday.
- Check which products need to be restocked.
- Receive deliveries and unpack boxes.
- Place products on the correct shelves.
- Organize and clean the store.
- Open the store to let customers in.
- Help customers find products.
- Complete daily tasks.
- Progress through the seasons.

## Functional requirements
### Player Control:
- The player can move around the store.
- The player can interact with objects.
- The player can pick up and place products.
- The player can unpack delivery boxes.
- The player can restock shelves.
- The player can clean dirty parts of the store.
- The player can interact with customers.
- The player can help customers find products.
- The player can help customers at the cash register.

### Season change:
- Seasonal decorations
- Seasonal items
- Old seasonal items have to be removed from the shelves in a simple way

### Environment:
- The store should change visually depending on the season.
- Seasonal decorations should appear throughout the store.
- Different areas of the store should have different types of products.
- The environment should be interactive where appropriate.
- The player should be able to buy new decorations, music and interactable things when the player progress through the game.

### Product System:
- Products should have different categories.
- Products should have specific locations where they can be placed.
- Products can be removed from shelves when purchased by customers.
- Empty shelves have to be restocked by the player.
- Seasonal products should become available during specific seasons.
- Some products should only be available during certain seasons.
- More products are being introduced when the game progress with new days. The store addapts and also becomes bigger overtime.

### Customer System:
- Customers should enter and leave the store.
- Customers can look for specific products.
- Customers can ask the player for help.
- Customers should display what product they are looking for.
- Customers should purchase available products.
- Different customers can request different products.
- Customers should pay at the end of their visit.
- Customers can't steal items so you will not lose items randomly. 

### UI System:
**The UI should display:**
- Current season
- Current day
- Current tasks
- Products that need restocking
- Customer requests
- Interaction prompts

## Non-functional requirements
### Performance
- The game should maintain a consistent frame rate with multiple customers and objects active simultaneously.

### Controls
- Controls should be responsive and easy to understand.
- Player movement should feel smooth.

### Balance
- More items should be introduced when the game progresses
- With new seasons the store will be revamped and keep it interesting with new items and things to look at.
- The game should remain relaxing rather than stressful.

### Visual Feedback
- The game should clearly communicate player actions.
- Easy to know where customers, shelves and the player are.