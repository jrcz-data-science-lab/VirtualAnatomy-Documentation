# `UCPP_TreeView.h`

The `UCPP_TreeView` class defines the tree view widget displayed in the sidebar, allowing users to interact with various components and hierarchical folders. This class manages the data structures needed to populate the tree view and provides helper methods for creating folders and assigning actors.

## Protected Methods

### `void NativePreConstruct()`

This Unreal Engine override is used for setup tasks before the widget is fully constructed. Any necessary initializations for the widget’s design and data binding can be set here.

### `void NativeConstruct()`

This Unreal Engine override handles initialization after the widget is constructed. The tree view’s data structures are populated, and event bindings are initialized at this stage.

## Protected Members

### `TObjectPtr<UTreeView> Tree`

The primary tree view component that displays the hierarchy in the sidebar. It is bindable and accessible within Blueprints, enabling a flexible and interactive UI for hierarchical data visualization.

## Private Members

### `TArray<UCPP_TreeViewEntry*> TreeViewEntries`

An array storing root-level entries in the tree view. Each `UCPP_TreeViewEntry` represents a folder or component within the sidebar tree structure.

## Private Methods

### `UCPP_TreeViewEntry* CreateTreeViewEntry(FString folderName, FName tag)`

**Parameters:**

- `FString folderName` - The name of the folder to be displayed in the tree view.
- `FName tag` - The tag associated with the folder, used to filter components.

**Returns:**

- `UCPP_TreeViewEntry*` - A pointer to the newly created folder entry.

This method creates a folder entry in the sidebar’s tree view based on the specified folder name and tag. It populates the entry’s data and appearance and returns the new entry, which can be further modified or populated with child components.

### `void AssignChildren(UCPP_TreeViewEntry* parent, FName tagName)`

**Parameters:**

- `UCPP_TreeViewEntry* parent` - The parent folder entry to which child actors will be assigned.
- `FName tagName` - The tag used to identify actors or components to be assigned as children.

This method performs a world search to find all actors associated with the specified tag and assigns them to the given parent folder entry. If the tag is set on components within an actor, the function will continue searching within that actor's components for matching items. This function helps populate the tree view with nested items and subcomponents dynamically based on the world context.
