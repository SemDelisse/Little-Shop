# TDD Little shop Game
### Project basics
I am going to use Unity 6.3 LTS (6000.3.10f1).

The project is going to be a 2D top down game.

The graphics are going to be 2D sprites.

For the UI system the standard simple Unity UI Toolkit and Canvas will be used.

Github is going to be used to save progress and to easily go back to a previous build or version of the game.

Builds are being made with the Unity Build Pipeline and being placed as download on github for windows 11.

### Player
#### Movement

The player controls are being made with the "new input system" of Unity. 

Movement will be controlled using keyboard input.

The player will move using a Rigidbody2D movement system.

The player should not be able to walk through walls, shelves or other solid objects. This will be done with box colliders and collision layers.

#### Interaction
The player can interact with different things in the game:
- Pick up products.
- Put products down.
- Open delivery boxes.
- Restock shelves.
- Empty shelves at beginning of season.
- Clean areas.
- Talk with customers.
- Help customers.
- Use the cash register.
- Interact with decorations and other shop objects.

The interaction system should use an interface such as IInteractable. This allows different objects to use the same interaction system.

By interaction with these different things the interaction system will do different things like open a new UI, play an animation, play a sound effect, change the music.

### Customer
#### Customer information
Customers are NPCs that enter the shop, look for products and purchase them.

Every customer will have a name, shopping list, small profile picture.

#### Customer States
Customers will use a simple state machine.

Possible states:
- Entering or coming in the shop
- Browsing
- LookingForProduct
- AskingForHelp
- GoingToShelf
- PickingUpProduct
- GoingToCheckout
- WaitingForCheckout
- Leaving
- Finished

Before entering the shop the system will decide what the customer is going to do. It will give the customer a shopping list with all the items it's going to buy. The system will also select if the customer is going to ask for help to search for a specific product.

#### Customer Requests
If it's decited that the customer is going to ask for help locating a specific product. A random product will be selected of their list and when the customer is going for that product it will stand still and be interactable for the player. Their will be visual that the customer needs help. When the player interacts with the customer it will follow the player and the place where he needs to be will be visualised. The player will also get a pop up message of what product the customer is searching for.

#### Customer Movement
Customers use Unity's navigation/pathfinding AI system for the game's 2D layout.
Customers should:
- Avoid walls.
- Avoid shop objects where possible.
- Avoid other customers.
- Move toward requested shelves.
- Move toward the checkout.
- enter through the shop entrance.
- Leave through the shop exist.

#### Purchasing
customer tip
When a customer finds a product:
- Customer checks whether the product is available.
- Product quantity is reduced.
- Customer adds the product to their shopping inventory.
- Customer continues shopping or goes to checkout.
- Customer pays at the checkout.
- A customer tip is calculated based on scenery, music and if the player helped the customer when needed.
- Customer leaves.
Customers should never randomly steal products.

### Products
Products are data-driven objects. A `ProductData` ScriptableObject can be used for this.

Each product should have information such as:
- Product ID
- Product name
- Category
  - Food
  - Drinks
  - Snacks
  - Household
  - Decorations
  - Seasonal
  - Etc
- Sprite
- Price
- Season availability
  - All year
  - Specific season
  - Specific year
- Shelf type
  - Dry
  - Cool
  - Freezer
  - Produce
  - Bakery
  - Display
  - Hanging
  - Locked/Restricted
- Storage location
  - Warehouse
  - Fridge
  - Freezer
- Maximum shelf quantity

#### Restocking
The restocking system is responsible for transferring products from shop storage to shelves and updating the corresponding inventory and visual states.
1. The player searches themself for a shelf with available restocking capacity.
2. The shelf checks its assigned ProductData and current stock level.
3. The player obtains the required product from storage.
4. The product is added to the player's carried-item state.
5. The player moves the product to the designated shelf.
6. The shelf validates that the product matches its allowed product type.
7. The shelf requests the product from the InventoryManager.
8. The inventory quantity is reduced.
9. The shelf quantity is increased.
10. The shelf visual is updated based on its new stock level.
11. A restocking event is triggered.
12. The TaskManager checks whether the action contributes to an active task.

The shelf should prevent restocking when:
- The product is incompatible with the shelf.
- The shelf is at maximum capacity.
- The product is unavailable in storage.
- The player is not within interaction range.
- The player doesn't have the product in the inventory.

#### Deliveries
The delivery system is responsible for adding new products to the shop's inventory.
- Deliveries are represented by interactive delivery box objects at the back of a delivery truck.
- A delivery is generated at the start of a workday.
- The delivery contains a predefined list of products and quantities.
- The delivery is spawned at the designated delivery location.
- The player interacts with the delivery box.
- The boxes are added to the appropriate storage location when the player interacts with the correct spot.
- The InventoryManager updates the stored quantity.
- The TaskManager receives a delivery event if an applicable task is active.
- When all boxes have been removed and stored correctly, the delivery is marked as completed by the TaskManager and the truck will leave.

### Inventory System
#### Player Inventory
The Player Inventory represents the products and objects currently carried by the player.

The player inventory should:
- Store products picked up by the player.
- Store the quantity of each carried product.
- Allow products to be added and removed.
- Prevent the player from carrying more than the defined capacity.
- Provide the carried product to shelves when restocking.
- Update the player's visual carried item when applicable.
- The player can only carry 1 box at a time, but can also move a cart with multiple boxes.
- ProductData reference

Removing Items

Products are removed when the player:
- Places a product on a shelf.
- Drops a product.
- Places a product into storage.
- Uses the product for another interaction.

#### Back Storage Inventory
The Back Storage Inventory represents all products currently stored in the shop's backroom/storage area. Unlike the Player Inventory, this inventory can contain large quantities of products.

The Back Storage Inventory should:
- Store delivered products.
- Track quantities for every product.
- Provide products for restocking.
- Receive products from deliveries.
- Receive products returned by the player.
- Support different storage locations.
  - Dry
  - Freezer
  - Cooling
- Persist between workdays.

The ProductData defines the required storage location.

Adding Stock
- The delivery system identifies the product.
- The InventoryManager checks the product's storage location.
- The product quantity is added to Back Storage.
- The delivery quantity is reduced.
- The storage UI/world representation is updated.

### Task System
The Task System manages daily objectives and tracks the player's progress.

#### Task Data
Tasks should be represented using TaskData ScriptableObjects containing:
- Task ID
- Task Name
- Task description
- Task type
  - Restock products
  - Unpack deliveries
  - Clean areas
  - Help customers
  - Sell products
  - Organize shelves
- Required amount
- Reward
- Availability conditions

#### Task Tracking
The TaskManager tracks active tasks and their progress.

Gameplay systems should send events to the TaskManager when relevant actions occur.

Tasks are completed when the required amount is reached and the player will receive the rewards for the task.

### Progression System
The Progression System controls the unlocking of new content as the player completes workdays.

Progression can unlock:
- Products
- Product categories
- Shelves
- Decorations
- Music
- Shop areas
- Customer types
- Shop upgrades

Progression should use a defined progression level or milestone system.

Unlocked content should be stored by the GameManager.

### Economy System / Tips
The Economy System manages the shop's money and customer payments.

#### Tips
Customer tips can be spent on:
- Decorations
- Music
- Shop expansions
- Shelves
- Upgrades
- Customer Payments

When a customer completes checkout:
- The customer's purchased products are calculated.
- Product prices are retrieved from ProductData.
- The total purchase value is calculated.
- A transaction event is generated.
- Tips

Tips are income generated by customer interactions to buy extra optional things with.

Tip values can depend on factors such as:
- Customer satisfaction
- Successfully helping customers
- Completing requests
- Store cleanliness

### Season System
Seasons:
- Spring
- Summer
- Autumn
- Winter

A season can define:
- Seasonal products
- Decorations
- Shop appearance
- Music
  - The standard shop music is in this theme. The player can always change the music that plays in a menu.
- Special tasks

Season data should preferably be stored using ScriptableObjects.
SeasonData contains
- Season name
- Background/visual settings
- Product list
- Decoration list
- Music
- Duration

#### Season Changes
At the end of a season:
- The current season is completed.
- Seasonal products are identified.
- Seasonal products are removed from shelves/storage.
- New seasonal products become available.
- Seasonal decorations are changed.
- Shop visuals are updated.
- The next season begins.

### Environment
The environment system manages the physical shop and its interactive objects.
Environment objects should use reusable prefabs where possible.

The environment consists of:
- Floors
- Walls
- Shelves
- Storage areas
- Checkout
- Decorations
- Doors
- Interactive objects

#### Shop Layout
The shop layout should be built using modular 2D tilemaps and prefabs.

The layout contains predefined areas such as:
- Entrance
- Sales Floor
- Checkout
- Back Storage
- Delivery Area
- Staff Area

These areas are mapped in the correct way and let the system know where customs can walk and how.

Each area can have its own interaction rules and product categories. Colliders should prevent the player and customers from passing through walls, shelves and other solid objects.

#### Shop Expansion
Shop expansions unlock additional physical areas.

When an expansion is unlocked:
- The expansion requirement is checked.
- The required progression is validated.
- The new area is enabled.
- New floor/wall tiles are activated.
- New interaction points are enabled.
- New shelves or storage locations become available.
- The expansion state is saved.

Expansions should preferably be implemented as separate prefabs or scene sections to avoid modifying the entire shop layout.

#### Cleaning
The cleaning system manages dirty areas in the shop.

A cleanable object should contain:
- IsDirty
- Cleaning duration
- Cleaning interaction
- Clean/dirty sprites or visual states

When the player cleans an object or location:
- The player enters interaction range.
- The cleaning interaction starts.
- A cleaning progress indicator is displayed.
- The required cleaning time is completed.
- IsDirty changes to false.
- The object's visual state changes.
- A cleaning event is sent to the TaskManager.

Dirty objects can be generated periodically or through predefined gameplay events.

### UI
The UI system displays gameplay information and provides feedback to the player. It should be divided into independent components.

#### Main HUD
Displays:
- Current day
- Current season
- Tips
- Active tasks
- Player inventory
- Interaction UI

Displays context-sensitive interaction prompts.

#### Task UI
Displays:
- Task description
- Current progress
- Completion state
- Rewards

#### Customer UI
Displays:
- Requested product
- Customer request icon
- Relevant customer information

UI elements should be updated through events rather than constantly checking game state every frame.

### Audio System
The Audio System manages music and sound effects.

#### Music
The system should support:
- Main shop music
- Seasonal music
- Menu music
- Event music

Music can change when the season changes.

#### Sound Effects
Sound effects should be triggered by gameplay events.
Examples:
- Picking up item
- Placing item
- Restocking
- Cleaning
- Opening delivery box
- Checkout
- Customer interaction
- UI interaction

Audio clips should be stored using configurable audio data rather than directly referenced throughout gameplay scripts.

An AudioManager should control:
- Music volume
- SFX volume
- Music playback
- SFX playback
- Audio transitions

### Visual Feedback
The Visual Feedback System communicates gameplay states to the player.

Visual feedback should be event-driven and should not require the player to inspect internal game data.

Examples:
- Products
- Shelf stock changes visually.
- New products appear when restocked.
- Empty shelves have a distinct visual state.
- Customers
- Requested products display as icons.
- Customers show when they require assistance.
- Checkout status is visually communicated.
- Tasks
- Progress updates immediately.
- Completed tasks receive a completed state.
- New tasks are visually highlighted.
- Interactions
- Interactable objects can display an outline, highlight or indicator.
- Interaction prompts appear when the player enters range.
- Environment
- Dirty objects have a visible dirty state.
- Seasonal decorations change with the current season.
- Newly unlocked areas can have a visual indication.

### Performance
The game should maintain a consistent frame rate with multiple customers and interactive objects.

Performance considerations:
- Avoid unnecessary Update() calls.
- Use object pooling for customers where appropriate.
- Use efficient collision layers.
- Keep sprite sizes appropriate.
- Avoid unnecessary Instantiate/Destroy operations during gameplay.
- Limit the number of active NPCs where necessary.