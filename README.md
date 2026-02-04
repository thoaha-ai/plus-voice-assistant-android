# Plus Voice Assistant (Android) — Template Project (ZIP)

This is a runnable Android Studio project template for a Google-Assistant-like voice assistant:
- Push-to-talk
- Always-on foreground service template
- Rule-based Bangla+English intents (Call/SMS/WhatsApp/Alarm/Timer/Date/Time)
- Room DB for speaker enrollment (5 users) + embeddings (template)
- Android TTS output

## IMPORTANT
This template includes:
- **FAKE STT** (placeholder text). You must plug in real STT (recommended: whisper.cpp JNI).
- **FAKE Speaker Embedding** (hash-based). You must plug in real speaker embedding (TFLite) for true voice ID.

The project is structured so you can replace those components without changing the rest.

## Build APK (Android Studio)
1. Open folder `PlusVoiceAssistant` in Android Studio.
2. Let Gradle sync.
3. Run on device (USB debug) or build:
   - Build > Build Bundle(s) / APK(s) > Build APK(s)

## Build APK (CLI)
- Debug APK:
  ./gradlew :app:assembleDebug
  Output:
  app/build/outputs/apk/debug/app-debug.apk

## Where to plug real models
### STT (Whisper)
- Replace `com.plus.voiceassistant.stt.SttEngine.Fake` with a JNI-backed engine:
  - Add NDK module under `app/src/main/cpp/`
  - Build whisper.cpp for Android
  - Expose `transcribePcm16()` via JNI

### Speaker Embedding (TFLite)
- Replace fake embedding in:
  - `SpeakerRecognizer.fakeEmbedding()`
  - `SpeakerEnrollmentManager` embedding generation
- Use a TFLite model that outputs an embedding vector (e.g., 192/256 dims)
- Then cosine match to stored vectors

## Permissions
- RECORD_AUDIO required
- READ_CONTACTS optional (for name->number)
- CALL_PHONE optional (auto-call); otherwise opens dialer
- SEND_SMS optional (auto-send); otherwise opens messaging app

## Next (Recommended v1.1)
- Implement voice confirmation state machine:
  - When action.requiresConfirm = true -> ask confirm -> listen yes/no -> execute
