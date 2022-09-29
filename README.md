<img align="right" src="https://media.discordapp.net/attachments/973891514021314560/1011723930781888562/book.png" height="140" width="140">

# Milk team
We are developing a core based on **[JDA](https://github.com/DV8FromTheWorld/JDA)**. It allows you to make the bot structure similar to Minecraft servers
# Milkshake core
## Module structure
### Module-info.json
A special file that will contain information about:
* Location of the main class of the module
* Module dependencies
* Soft dependencies (optional dependencies)
* Tables that the database initializes automatically
```json
{
	"name": "EconomyModule",
	"main-class": "ru.milkshake.modules.economy.EconomyModule",
	"user-database-fields": {
		"balance": "BIGINT default 0"
	},
	"dependencies": [
		"PermissionModule",
		"CoolDownModule"
	]
}
```
### Module main class
```java
public class MilkModule extends JavaModule {

	private static MilkModule instance;

	public static MilkModule getInstance() {
		return instance;
	}

	@Override
	public void onLoad() {
		instance = this;
		// Initial loading of the module (modules are loaded first, and then all are turned on in turn)
	}

	@Override
	public void onEnable() {
		// Module enabling
	}

	@Override
	public void onSave(@NotNull SaveReason reason) {
	}

	@Override
	public void onDisable() {
	}
	// and other methods
}
```
## JSON configurations
Just a handy utility for JSON configs. For other extensions, you can use the AbstractConfig class
```java
public class MilkConfig extends JsonConfig {
	
	private String variable;
	
	public MilkConfig(@NotNull JavaModule module, @NotNull String fileName) {
		super(module, fileName);
		this.load();
	}
	
	public String getVariable() {
		return variable;
	}

	@Override
	public void load(@NotNull JsonElement element) {
		JsonObject data = element.getAsJsonObject();
		this.variable = data.get("variable").getAsString();
	}
}
```
## Manager classes
We try to make managers non-static to avoid some problems with modules. Also, our managers themselves call the methods of loading/shipping/saving (with general saving)
```java
public class MilkManager extends AbstractManager {
	
	private List<String> list = new ArrayList<>();

	@Override
	protected void onLoad() {
		this.list.addAll(List.of("1", "2", "3"));
	}

	@Override
	protected void onUnload() {
		this.list.clear();
	}

	@Override
	protected void onSave() {
		// Save to database maybe?
	}
}
```
```java
public static void main(String[] args) {
	MilkManager manager = new MilkManager();
	manager.load(); // Module load
}
```
## Task and timers
```java
public static void runTasks() {
	MilkRunnable task = new MilkRunnable() {
		@Override
		public void run() {
			System.out.println("Hello world");
		}
	};
	task.runTaskLater(10, TimeUnit.SECONDS); // Output after 10 seconds
	task.runTaskAt(Calendar.getInstance()); // Output in date
	task.runTaskTimer(10, TimeUnit.SECONDS); // Output every 10 seconds
}
```
### Async tasks
```java
public static void runAsyncTask() {
	Milkshake.getTaskManager().runAsync(() -> System.out.println("Async runnable"));
}
```
