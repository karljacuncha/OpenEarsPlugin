# OpenEars Plugin

The Javascript side is managed through (literate) Coffeescript, just because I like that sort of thing
http://coffeescript.org/#literate
You can safely ignore/delte this file and go straight to the JS [here](OpenEars.js) if you like, but this should be more
readable for getting started and finding out what's going on.

Again, names normalised for re-use on other platforms/swapping of engines


	cordova = require("cordova")
	exec = require("cordova/exec")

	class OpenEars

## Initial Set up

Communication to the calling app will all be handled with event triggers.
We don't have jquery here, so raw javascript custom events...

		trigger = (evt, args) ->
			document.dispatchEvent new CustomEvent(evt, {detail: args})

Just to be lazy, here's a generic error handler:
If anything bad happens, just trigger an error with the message and let the calling app decide what to do with it.

		genericHandleError = (args) ->
			trigger "openEarsError", args


		startAudioSession: () ->
			success = (args) ->
				trigger "startAudioSession", args
			exec(success, genericHandleError, "OpenEars", "startAudioSession", [])


## Language Generator:
			
Generate a language model from a CSV list of words to phrases to recognise, eg:
START,STOP,LEFT,RIGHT
The callback args is a dict of the language model & dictionary path & filenames.
eg:
{
	DictionaryFile: "FirstOpenEarsDynamicLanguageModel.dic",
	DictionaryPath: " *[path to your app]* /Library/Caches/FirstOpenEarsDynamicLanguageModel.dic",
	LMFile: "FirstOpenEarsDynamicLanguageModel.DMP",
	LMPath: " *[path to your app]* /Library/Caches/FirstOpenEarsDynamicLanguageModel.DMP"
}


		generateLanguageModel: (languageName, languageCSV) ->
			success = (args) ->
				trigger "generateLanguageModel", args
			exec(success, genericHandleError, "OpenEars", "generateLanguageModel", [languageName, languageCSV.toUpperCase() ])

## Pocket Sphinx:

		stopListening: () ->
			success = (args) ->
				trigger "stopListening", args
			exec(success, genericHandleError, "OpenEars", "stopListening", [])

		resumeListening: (options) ->
			success = (args) ->
				trigger "resumeListening", args
			exec(success, genericHandleError, "OpenEars", "resumeListening", [])

		suspendRecognition: () ->
			success = (args) ->
				trigger "suspendRecognition", args
			exec(success, genericHandleError, "OpenEars", "suspendRecognition", [])

		resumeRecognition: () ->
			success = (args) ->
				trigger "resumeRecognition", args
			exec(success, genericHandleError, "OpenEars", "resumeRecognition", [])
			
#### Multi-Language swapping

		startListeningWithLanguageModelAtPath: (languagemodel, dictionary, options=null) ->
			success = (args) ->
				trigger "startListeningWithLanguageModelAtPath", args
			exec(success, genericHandleError, "OpenEars", "startListeningWithLanguageModelAtPath", [languagemodel, dictionary, options])

		changeLanguageModelToFile: (languagemodel, dictionary, options=null) ->
			success = (args) ->
				trigger "changeLanguageModelToFile", args
			exec(success, genericHandleError, "OpenEars", "changeLanguageModelToFile", [languagemodel, dictionary, options])

## Flite, Text to Speech

		say: (phrase) ->
			success = (args) ->
				trigger "say", args
			exec(success, genericHandleError, "OpenEars", "say", [phrase])


		events: {
			startedListening: () ->
				trigger "startedListening"

			stoppedListening: () ->
				trigger "stoppedListening"

			detectedSpeech: () ->
				trigger "detectedSpeech"

			finishedDetectingSpeech: () ->
				trigger "finishedDetectingSpeech"

			suspendedRecognition: () ->
				trigger "suspendedRecognition"

			resumedRecognition: () ->
				trigger "resumedRecognition"

			startedCalibration: () ->
				trigger "startedCalibration"
				
			finishedCalibration: () ->
				trigger "finishedCalibration"

			continuousSetupDidFail: () ->
				trigger "continuousSetupDidFail"

			testRecognitionCompleted: () ->
				trigger "testRecognitionCompleted"

			receivedHypothesis: (hypothesis, recognitionScore, utteranceID) ->
				trigger "receivedHypothesis", {
												hypothesis: hypothesis
												recognitionScore: recognitionScore
												utteranceID: utteranceID
												}

			changedLanguageModelToFile: (modelPath, dictionaryPath) ->
				trigger "changedLanguageModelToFile", {
														modelPath: modelPath
														dictionaryPath: dictionaryPath
														}

			startedSpeaking: () ->
				trigger "startedSpeaking"

			finishedSpeaking: () ->
				trigger "finishedSpeaking"


			audioSessionInterruptionDidBegin: () ->
				trigger "audioSessionInterruptionDidBegin"
				
			audioSessionInterruptionDidEnd: () ->
				trigger "audioSessionInterruptionDidEnd"
				
			audioInputDidBecomeUnavailable: () ->
				trigger "audioInputDidBecomeUnavailable"
				
			audioInputDidBecomeAvailable: () ->
				trigger "audioInputDidBecomeAvailable"
				
			audioRouteDidChangeToRoute: () ->
				trigger "audioRouteDidChangeToRoute"
				
		}

	OpenEars = new OpenEars()
	module.exports = OpenEars
