# SaveEngine

### Save and load game mechanics with slots and autosave.

## Services:

### SaveService

It's the main singleton service of the plugin, used to save, load, delete and list save slots.

Save files will be validated by hash and salt.
It will save singleton classes as global data.
Which global classes must be saved can be configured on the auto generated file: Data/Settings/saveSettings.cfg

**Methods:**

* **NewSlot(slotId : String = "", name : String = "", currentScene : String = "", metaData : Dictionary = {}) -> bool**
    * Create a new save slot 

* **DeleteSlot(slotId : String) -> bool:**
    * Delete an saved slot

* **SaveGame(autoSetCurrentScene = true) -> bool**
    * Save a loaded game slot

* **LoadGame(slotId : String) -> bool**
    * Load a game slot

* **GetSlots() -> Array[SaveFile]**
    * Get all saved slots (files)

### Configuration File:

```json
{
    "HashSalt":"HASH salt (used to validate save files, can be any text or hash)",
    "SaveFilesPrefix":"prefix to add to every save file, can be empty",
    "SaveFolderPath":"default save file folder, default: user://Saves/",
    "StaticScriptsToSave":["List of singleton class to be saved as global data, if empty will save all singletons (no recommended)"]
}
```

### SaveAgent

Must be placed on a scene that pretend to be saved, handles all saving process for the scene, there's no need to call SaveService methods on the scene.
If there’s an loaded slot and in this slot there’s data related to the scene, will automatically apply the saved data to the nodes that contains the SaveElement child.

![image](images/doc1.png)

**Signals:**

* **StartAutoSaving**
* **FinishAutoSaving**
* **StartSaving**
* **FinishSaving**
* **StartLoading**
* **FinishLoading**

**Properties:**

* **AutoSave : bool = true**
    * Enable autosave
* **AutoSavePeriod : int = 60**
    * Auto save period in seconds

**Methods:**

* **LoadSceneData()**
    * Manually trigger the load scene data

* **SaveSceneData(auto = false)**
    * Manually trigger the save scene data

### SaveElement2D and SaveElement3D

Must be placed on a node or scene that pretends to have the properties saved, handles the serialization and load of the object.
Used by the SaveAgent to save and load the scene elements.
Specific properties to be saved can be set on the inspector.

![image](images/doc2.png)
![image](images/doc3.png)

**Properties:**

* **SaveTransform : bool = true**
    * Save the transform properties of the parent node
* **SaveVisibility : bool = true**
    * Save the visibility properties of the parent node
* **SaveProperties : Array[String] = []**
    * List of properties to be saved.
    * Can save child nodes properties, use following pattern: "nodePath:property" 
        ex:. "geometry:rotation"

**Methods (used internally be SaveAgent):**

* **Load(data : SaveFileNode)**
    * Load parent properties

* **Serialize() -> SaveFileNode**
    * Serialize parent properties

### About

By Cianci 

KelvysB.

Check Cianci Tutorials (Brazilian Portuguese):

https://www.youtube.com/@CiaNCIStudio

