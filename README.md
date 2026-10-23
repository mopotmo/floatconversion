# floatconversion

A Java Sound `FormatConversionProvider` that adds a `PCM_FLOAT` encoding (samples as 32-bit float or 64-bit double, normalized to ±1.0) and converts between it and `PCM_SIGNED`. It was written in 2004 as part of the diploma thesis behind the [GrooveManager MC-909 Sample Editor](https://github.com/mopotmo/groovemanager-mc909se). Pure Java, so it works on every platform.

> **Status: archive.** These are the original sources of release 1.0 (2004-07-16).

Used by:
- [jsREX](https://github.com/mopotmo/jsrex), which delivers ReCycle audio as `PCM_FLOAT`, so this plugin is required
- [jsASIO](https://github.com/mopotmo/jsasio), because some ASIO drivers work with float samples
- the GrooveManager MC-909 Sample Editor

## Usage

Put `floatconversion.jar` on the classpath. It registers `com.groovemanager.spi.floatEncoding.FloatConversionProvider` through `META-INF/services`, after which `AudioSystem.getAudioInputStream(targetFormat, stream)` can convert between `PCM_SIGNED` and `PCM_FLOAT`. `FloatAudioFormat` provides convenience constructors:

```java
AudioFormat f = new FloatAudioFormat(44100f, 32, 2, true);   // 32-bit float, stereo, big endian
```

Conversion works in four steps: read the integer samples, fix the byte order, pad to the target sample size, then convert each sample between integer and float (thesis, section VI.1.1.2).

## Historical note

Java Sound had no float encoding in 2004. Java 7 added `AudioFormat.Encoding.PCM_FLOAT` together with built-in float conversion. This plugin uses the same encoding name, and Java Sound compares encodings by name, so on a current JVM the plugin is probably no longer needed. This hasn't been verified yet.

## License

MIT, see [LICENSE](LICENSE).
