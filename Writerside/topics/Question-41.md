# Вопрос 41

## Адаптер {collapsible="true"}

Адаптер — это структурный паттерн проектирования, который позволяет объектам с несовместимыми интерфейсами работать вместе.

![image_18.png](image_18.png)

```Kotlin
// Интерфейс для воспроизведения аудиофайлов
interface AudioPlayer {
    fun playAudio(filename: String)
}

// Класс, реализующий воспроизведение аудиофайлов в формате MP3
class Mp3Player : AudioPlayer {
    override fun playAudio(filename: String) {
        println("Playing MP3 audio file: $filename")
    }
}

// Класс, представляющий аудиофайл в формате AAC
class AacFile(private val filename: String) {
    fun play() {
        println("Playing AAC audio file: $filename")
    }
}

// Адаптер для воспроизведения аудиофайлов в формате AAC через интерфейс AudioPlayer
class AacAdapter(private val aacFile: AacFile) : AudioPlayer {
    override fun playAudio(filename: String) {
        aacFile.play()
    }
}

// Пример использования паттерна Адаптер
fun main() {
    val mp3Player: AudioPlayer = Mp3Player()
    val aacFile = AacFile("audio.aac")
    val aacAdapter: AudioPlayer = AacAdapter(aacFile)

    mp3Player.playAudio("audio.mp3") // Вывод: Playing MP3 audio file: audio.mp3
    aacAdapter.playAudio("audio.aac") // Вывод: Playing AAC audio file: audio.aac
}
```

## Мост {collapsible="true"}

Мост — это структурный паттерн проектирования, который
разделяет один или несколько классов на две отдельные
иерархии — абстракцию и реализацию, позволяя изменять
их независимо друг от друга.

![image_19.png](image_19.png)

```Kotlin
// Абстракция пульта управления
abstract class RemoteControl(protected val device: Device) {
    abstract fun powerOn()
    abstract fun powerOff()
    abstract fun volumeUp()
    abstract fun volumeDown()
}

// Реализация устройства
interface Device {
    fun powerOn()
    fun powerOff()
    fun volumeUp()
    fun volumeDown()
}

// Конкретная реализация устройства: Телевизор
class Television : Device {
    override fun powerOn() {
        println("TV: Power On")
    }

    override fun powerOff() {
        println("TV: Power Off")
    }

    override fun volumeUp() {
        println("TV: Volume Up")
    }

    override fun volumeDown() {
        println("TV: Volume Down")
    }
}

// Конкретная реализация устройства: Радио
class Radio : Device {
    override fun powerOn() {
        println("Radio: Power On")
    }

    override fun powerOff() {
        println("Radio: Power Off")
    }

    override fun volumeUp() {
        println("Radio: Volume Up")
    }

    override fun volumeDown() {
        println("Radio: Volume Down")
    }
}

// Конкретная реализация пульта: Простой пульт управления
class SimpleRemoteControl(device: Device) : RemoteControl(device) {
    override fun powerOn() {
        device.powerOn()
    }

    override fun powerOff() {
        device.powerOff()
    }

    override fun volumeUp() {
        device.volumeUp()
    }

    override fun volumeDown() {
        device.volumeDown()
    }
}

// Конкретная реализация пульта: Расширенный пульт управления
class AdvancedRemoteControl(device: Device) : RemoteControl(device) {
    override fun powerOn() {
        device.powerOn()
        println("AdvancedRemoteControl: Additional actions for power on")
    }

    override fun powerOff() {
        device.powerOff()
        println("AdvancedRemoteControl: Additional actions for power off")
    }

    override fun volumeUp() {
        device.volumeUp()
        println("AdvancedRemoteControl: Additional actions for volume up")
    }

    override fun volumeDown() {
        device.volumeDown()
        println("AdvancedRemoteControl: Additional actions for volume down")
    }
}

// Пример использования паттерна Мост
fun main() {
    val television: Device = Television()
    val radio: Device = Radio()

    val simpleRemoteControlForTV: RemoteControl = SimpleRemoteControl(television)
    val advancedRemoteControlForRadio: RemoteControl = AdvancedRemoteControl(radio)

    simpleRemoteControlForTV.powerOn() // Вывод: TV: Power On
    simpleRemoteControlForTV.volumeUp() // Вывод: TV: Volume Up

    advancedRemoteControlForRadio.powerOn() // Вывод: Radio: Power On
    advancedRemoteControlForRadio.volumeDown() // Вывод: Radio: Volume Down
}
```