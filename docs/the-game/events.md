---
sidebar_position: 2
---

# Events

Events are handled in asynchronous way.

## Click

When you click on a object in the game, "merge-game" file will handle this, and at a certain point it will call this:

```js
this._listener.onTileEvent("CityInteractionEvent/TILE_CLICKED", a);
```

"a" variable is an object containing x and y coordinates of the clicked tile.

On tile event is like this:

```js
_onTileEvent: function(a, b) {
            b = null != b ? this._snakeIsoEntityContainer.getIsoTile(b.x | 0, b.y | 0) : null;
            this._cityModel.get_entitiesModeController().processTileEvent(a, b);
            "CityInteractionEvent/TILE_CLICKED" != a && "CityInteractionEvent/TILE_OVER" != a || this.dispatch(new wl(a,b))
        },
```

a little more clear:

```js
_onTileEvent: function(eventType, position) {
    // Get the isoTile based on the provided position
    // Convert position.x and position.y to integers using bitwise OR to truncate decimal parts
    position = position !== null ? this._snakeIsoEntityContainer.getIsoTile(position.x | 0, position.y | 0) : null;
    
    // Get the controller responsible for handling entity modes in the city model
    const entitiesModeController = this._cityModel.get_entitiesModeController();
    
    // Process the tile event using the retrieved controller and the isoTile
    entitiesModeController.processTileEvent(eventType, position);
    
    // Check if the eventType indicates a clicked or hover event
    if (eventType === "CityInteractionEvent/TILE_CLICKED" || eventType === "CityInteractionEvent/TILE_OVER") {
        // If it's a clicked or hover event, dispatch a new event with the eventType and isoTile position
        this.dispatch(new wl(eventType, position));
    }
}
```

Then processTileEvent is called:

```js
processTileEvent: function(a, b) {
    this._processEvent(a, b)
}
```

That calls _processEvent:

```js
_processEvent: function(a, b) {
    if ("CityInteractionEvent/TILE_CLICKED" != a || !this._blockTileClickEvents) {
        var c = null;
        null != b && (c = b.entity);
        this._lastInteractable = this._getInteractable(c);
        null != this._currentMode && this._callHandler(a, this._lastInteractable, b)
    }
}
```

a little more clear:

```js
_processEvent: function(eventType, position) {
    // Check if the event type is "CityInteractionEvent/TILE_CLICKED" and tile click events are not blocked
    if (eventType !== "CityInteractionEvent/TILE_CLICKED" || !this._blockTileClickEvents) {
        // Initialize a variable 'c' to store an entity based on the provided position
        var entityFromPosition = null;
        if (position !== null) {
            entityFromPosition = position.entity;
        }
        
        // Get the interactable associated with the entity from the provided position
        this._lastInteractable = this._getInteractable(entityFromPosition);
        
        // Check if the current mode is not null and call the handler with the event type, interactable, and position
        if (this._currentMode !== null) {
            this._callHandler(eventType, this._lastInteractable, position);
        }
    }
}
```
