
## Material Design in Android Dev
Key features of Material Design include:

### 1. Material Metaphor: This concept uses tactile surfaces and realistic lighting to create a sense of depth. Elements look and behave like physical materials, providing a familiar experience for users.
Example: To make a gradient background for app
```
@Composable
fun GradientBackground() {
    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(
                brush = Brush.horizontalGradient( 
                    // use Brush.verticalGradeint to change the angle
                    colors = listOf(
                        Color(0xFF64B5F6), // Start color
                        Color(0xFF0D47A1)  // End color
                    )
                )
            )
    )
}
```
link: https://developer.android.com/reference/kotlin/android/graphics/LinearGradient

### 2. Typography: Bold, Graphic, and Intentional: It emphasizes bold colors, large typography, and intentional white space to create a visually appealing and easy-to-navigate interface.
Example:

Create a Font Resource Folder:

In your res directory, create a new folder named font if it doesn't already exist. Right-click on res, go to New > Android Resource Directory, select font as the Resource Type, and click OK.

Add Your Font File:

Place your .ttf or .otf font files into the res/font directory.
```
@Composable
fun Greeting(name: String) {
    // Create a FontFamily using the custom font
    val customFont = FontFamily(
        Font(R.font.your_custom_font) // Replace with your font file name
    )

    Text(
        text = name,
        fontSize = 24.sp,
        fontFamily = customFont // Apply the custom font
    )
}
```

### 3. Motion Provides Meaning: Animations are used to guide users, indicate actions, and provide feedback, enhancing the overall experience rather than distracting from it.
Example:
First to add animation dependency in build.gradle file
```
dependencies {
    implementation "androidx.compose.animation:animation:1.3.0"
    // Other dependencies
}
```
simple fade-in animation using Jetpack Compose:
```
@Composable
fun AnimatedContent() {
    val visible = remember { mutableStateOf(false) }

    Button(onClick = { visible.value = !visible.value }) {
        Text("Toggle Animation")
    }

    AnimatedVisibility(
        visible = visible.value,
        enter = fadeIn()
    ) {
        Text("Hello, Animated World!")
    }
}
```



### 4. Adaptive Design: Material Design promotes responsive and adaptive layouts that adjust to various screen sizes and orientations, ensuring a consistent experience across devices.
Example: LazyColumn
First set up dependencies:
```
dependencies {
    implementation "androidx.compose.ui:ui:1.3.0"
    implementation "androidx.compose.material3:material3:1.0.0"
    implementation "androidx.compose.ui:ui-tooling-preview:1.3.0"
    // Other dependencies
}
```
code for lazy column
```
@Composable
fun ItemList() {
    // Sample data
    val items = List(100) { "Item #$it" }

    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        contentPadding = PaddingValues(16.dp)
    ) {
        items(items) { item ->
            ItemCard(item)
        }
    }
}

@Composable
fun ItemCard(item: String) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 4.dp)
            .clickable { /* Handle click event */ },
        elevation = CardDefaults.elevation(4.dp)
    ) {
        Row(
            modifier = Modifier.padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Text(text = item, fontSize = 20.sp)
        }
    }
}
```
### 5. Components and Patterns: It includes a rich library of UI components and design patterns, making it easier for developers to create consistent and functional applications.
Example: Modifier

——————

## Basic Layout in Android Compose
Link: https://developer.android.com/develop/ui/compose/layouts/basics
Column
```
Column {
    Text("Item 1")
    Text("Item 2")
}
```

Row
```
Row {
    Text("Item 1")
    Text("Item 2")
}
```

Box
```
Box {
    Image(painterResource(R.drawable.image), contentDescription = null)
    Text("Overlay Text", modifier = Modifier.align(Alignment.Center))
}
```
check mark example:
```
@Composable
fun ArtistAvatar(artist: Artist) {
    Box {
        Image(bitmap = artist.image, contentDescription = "Artist image")
        Icon(Icons.Filled.Check, contentDescription = "Check mark")
    }
}
```

Modifier
```
Text("Hello", modifier = Modifier.padding(16.dp))
```

Spacer
```
Row {
    Text("Item 1")
    Spacer(modifier = Modifier.width(16.dp))
    Text("Item 2")
}

```

LazyColumn
```
LazyColumn {
    items(listOf("Item 1", "Item 2", "Item 3")) { item ->
        Text(item)
    }
}
```

Card
```
Card {
    Text("This is a card")
}
```

!!! Scaffold
1. Basic Layout Structure: Scaffold sets up the fundamental layout for your UI, including slots for the app bar, bottom navigation, floating action button, and the main content.

2. Top App Bar: You can easily add a TopAppBar to the scaffold, which is typically used for displaying the title of the screen and actions like search or settings.

3. Bottom Navigation: It allows you to include bottom navigation components, making it easy to switch between different sections of your app.

4. Floating Action Button (FAB): You can add a FAB that typically performs the primary action of the screen, such as adding a new item.

5. Content Area: The main content of the screen can be placed within the Scaffold, and it handles the padding automatically to accommodate the app bar, navigation bar, and FAB.
```
Scaffold(
    topBar = { TopAppBar(title = { Text("Title") }) },
    floatingActionButton = { FloatingActionButton(onClick = {}) { Icon(Icons.Filled.Add, contentDescription = null) } }
) { innerPadding ->
    // Content goes here
}
```
```
@Composable
fun MyApp() {
    MaterialTheme {
        Scaffold(
            topBar = {
                TopAppBar(
                    title = { Text("My App") },
                    actions = {
                        IconButton(onClick = { /* Handle action */ }) {
                            Icon(Icons.Default.Search, contentDescription = "Search")
                        }
                    }
                )
            },
            floatingActionButton = {
                FloatingActionButton(onClick = { /* Handle FAB click */ }) {
                    Icon(Icons.Default.Add, contentDescription = "Add")
                }
            }
        ) { paddingValues ->
            // Main content goes here
            // Use paddingValues to avoid content overlapping with top bar or FAB
            Text(
                text = "Hello, Scaffold!",
                modifier = Modifier.padding(paddingValues)
            )
        }
    }
}

```
