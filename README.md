# Agora Content Inspect Moderation Showcase

This GitHub project showcases the content inspect moderation features available with Agora and their partner ActiveFence. It demonstrates how to enable and use the Agora Content Inspector to moderate content in real-time during Agora video calls.

## Prerequisites

Before getting started, ensure that you have the following:

- An Agora developer account. If you don't have one, you can create an account at the [Agora Developer Console](https://console.agora.io).
- Android Studio
- Basic knowledge of Kotlin

## Installation

To run the project locally, follow these steps:

1. Clone the repository to your local machine:

```bash
git clone https://github.com/Meherdeep/Agora-ActiveFence-Android
```

1. Open the project in Android Studio.
2. Replace <--Add Your AppId here--> in `MainActivity.kt` with your Agora App ID. You can obtain an App ID by creating a project at the Agora Developer Console.
3. Enable ActiveFence moderation on your Agora Account (not yet available)
4. Optional: Set up Kicking Server
    1. Head to [agora-activefence-kicker](https://github.com/AgoraIO-Community/agora-activefence-kicker) and use the one-click deployment for the server
    2. Add the deployment link + `/kick/` to the webhook URL for the desired event in the ActiveFence dashboard.
5. Build and run the project on an Android device.

## Usage

Focusing on only the content moderation portions, this example app completes the following steps:

### Initialise the `ContentInspectConfig`

The inspect config is set up inside `ContentInspectModule` as such:

```kotlin
   val contentInspectModule = ContentInspectModule()
   contentInspectModule.type = ContentInspectConfig.CONTENT_INSPECT_TYPE_IMAGE_MODERATION
   contentInspectModule.interval = 2
   val contentInspectConfig = ContentInspectConfig()
   contentInspectConfig.modules[0] = contentInspectModule
   contentInspectConfig.moduleCount = 1
```

The above config monitors the images in a video feed, taking one frame every 5 seconds.

### Apply `ContentInspectConfig` To The Engine

The content inspect should be enabled after joining a channel.

```kotlin
    val returnVal = agoraEngine?.enableContentInspect(true, contentInspectConfig)
```

### Catch Banning Events

To know when your local user is banned from the scene, you can utilise the `connectionStateChangedTo` delegate method:

```kotlin
    override fun onConnectionStateChanged(state: Int, reason: Int) { 
        super.onConnectionStateChanged(state, reason)
        if (state == Constants.CONNECTION_STATE_FAILED && reason == Constants.CONNECTION_CHANGED_BANNED_BY_SERVER ) {
            showMessage("Stream banned by the server")
        }
    }
```

> This will only happen if you have applied the `Kicking Server` steps above, or created your own kicking server that communicates with ActiveFence.

## Contributing

Contributions to this project are welcome. If you find any issues or have suggestions for improvements, please feel free to open a GitHub issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).