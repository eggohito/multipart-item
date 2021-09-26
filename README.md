# Item Multi-part Library
This datapack uses two of the [MinecraftPhi](https://github.com/MinecraftPhi/MinecraftPhi-modules) modules (mostly `phi.modifyinv` and `phi.core`) in order to modify player inventories indirectly.
<br>

This datapack also uses [Lantern Load](https://github.com/LanternMC/Load) to ensure that this datapack loads before your datapack. You can detect if this library is loaded by checking the score of the `item-multi-part-lib` score holder from the `load.status` scoreboard objective.
<br>

e.g: `100` = Version 1.0.0, `110` = Version 1.1.0, etc.
<br>
<br>

You would reference your load function inside the `#load:post_load` function tag (`data/load/tags/functions/post_load.json`) after copying the `load` folder from Lantern Load's repository.
```json
{
    "values": [
        "<namespace>:<path/to/function>"
    ]
}
```
* `<namespace>` being the folder you're using inside the `data` folder
* `<path/to/function>` being your load function
<br>

## How to use:
This datapack uses a general item modifier for declaring an item stack to have a part added. You can use your own item modifier to specify what kind of part you'll add to the specified item stack.

<br>

<details>
<summary>
Create a power that uses the <code>origins:item_on_item</code> power type (for Origins use)
</summary>
<br>

Upon creating the power, you would check for your "part" item inside its `using_item_condition` item condition object. 

You can then check for the "base" item that'll have a part added to it once you right-click the said item with the "part" item in its `on_item_condition` item condition object. 

"Base" items store its part count, parts, and other data in its `item-multi-part-lib` NBT path that you can check for in the <code>on_item_condition</code> item condition object.

Afterwards, you can run any item action types in its `using_item_action` item action object. You would then run an `origins:modify` item action type inside its `on_item_action` item action object to apply an item modifier to the "base" item, in order to add a part to it.

Finally, you would run an "add part" function with the `origins:execute_command` entity action type in its `entity_action` entity action object that would modify the "base" item to have the "part" item data declared from the function to the "base" item.

[Here's a full example that would only add a "part" to the "base" item if the "base" item doesn't have more than 4 parts, and it has the `origins-amulet: 1b` NBT](https://github.com/eggohito/item-multi-part-lib/blob/1.17.x/example/data/origins-amulet/powers/parts_on_amulet.json)

<br>
</details>

<details>
<summary>
Create an "add part" function for adding "part" or arbitrary data to the "base" item
</summary>
<br>

In the function, you would need to append any data type to the `input[{tag: {item-multi-part-lib: {add_part: {new: 1b, id: "<id>"}}}}].tag.item-multi-part-lib.parts` NBT array (but you can only have single data type in the array), doing so would increase the "base" item's `item-multi-part-lib.part_count` NBT, which can then be used to check how many parts are there in the "base" item. 

After putting in your "part" data and/or arbitrary data in the `input` NBT path of the `item-multi-part-lib:io` storage, you can call the `item-multi-part-lib:api/set_data` function to apply the changes made to the "base" item.

[Here's an example function that would append an NBT compound in the `[{tag: {item-multi-part-lib: {add_part: {new: 1b, id: "diamond"}}}}].tag.item-multi-part-lib.parts` NBT path containing the generic data of the "part" item](https://github.com/eggohito/item-multi-part-lib/blob/1.17.x/example/data/origins-amulet/functions/add_part/diamond.mcfunction)

</details>