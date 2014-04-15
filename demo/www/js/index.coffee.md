
# Demo App for OpenEars on Cordova

Requires jQuery


Wait for documet & device ready...

	$(document).on "ready", () ->
		$(document).on "deviceready", () ->
			$(".event.listening").hide()
			$(".event.received").show()			

Update Status: simple logging of changes to a textarea, prepend new status updates to the top.

			$status =  $("#status")
			updateStatus = (new_status) ->
				previous_statuses = $status.html()
				$status.html(new_status + "\n" + previous_statuses)
				
			$(document).on "openEarsError", (evt) ->
				updateStatus "openEarsError"
				updateStatus evt.originalEvent.detail

			updateStatus("Starting App...")

### Initial setup: start the audio session and generate a language list

			Ears = cordova.plugins.OpenEars
			Ears.startAudioSession()
			
			$(document).on "startAudioSession", (evt) ->
				updateStatus "Audio Session Started..."
				
			window.languages = {}		# a list of generated langs, 
			languages["directions"] = {}
			languages["directions"].name = "Directions"
			languages["directions"].csv = "BACKWARD,CHANGE,FORWARD,GO,LEFT,MODEL,RIGHT,TURN"
			languages["directions"].paths = {}  # to be retrieved after generation
			Ears.generateLanguageModel(languages["directions"].name, languages["directions"].csv)

			$(document).on "generateLanguageModel", (evt) ->		
				languages["directions"].paths = evt.originalEvent.detail
				updateStatus "generate Language Model"



### Speech Recognition: Set up UI cointrols & handlers:

Basic buttons for start/stop listening:
				
			$start_listening 		= $("#start_listening")
			$stop_listening 		= $("#stop_listening")
			$suspend_recognition	= $("#suspend_recognition")
			$resume_recognition 	= $("#resume_recognition")

			$resume_recognition.hide()
			$start_listening.hide()
			
			$stop_listening.on "click", (e) ->
				Ears.stopListening()
				$start_listening.show()
				$stop_listening.hide()
				$resume_recognition.hide()
				$suspend_recognition.hide()

			$start_listening.on "click", (e) ->
				Ears.resumeListening()
				$start_listening.hide()
				$stop_listening.show()
				$resume_recognition.hide()
				$suspend_recognition.show()

			$suspend_recognition.on "click", (e) ->
				Ears.suspendRecognition()
				$resume_recognition.show()
				$suspend_recognition.hide()

			$resume_recognition.on "click", (e) ->
				Ears.resumeRecognition()
				$resume_recognition.hide()
				$suspend_recognition.show()

				
When a word is recognised, just display...

			$heard = $("#heard")
			updateHeard = (detail) ->
				# detail: {hypothesis, recognitionScore, utteranceID}
				$heard.html(detail.hypothesis)
								  
			$(document).on "receivedHypothesis", (evt) ->
				updateStatus "received Hypothesis"
				# data is in original event, not the jquery wrapper...
				detail = evt.originalEvent.detail
				updateHeard detail



### Text to Speech:

			$speak 			= $("#speak")
			$words_to_say	= $("#words_to_say")

			$speak.on "click", (e) ->
				Ears.say $words_to_say.val()




### Other Event listeners:

These are all just dummy status updates here.
It's unlikely you'll use many (or any?) of these, but they're included here for completeness...


			$(document).on "stopListening", (evt) ->
		  		updateStatus "stop Listening"

			$(document).on "stoppedListening", (evt) ->
		  		updateStatus "stopped Listening"

			$(document).on "resumeListening", (evt) ->
		  		updateStatus "resume Listening"

			$(document).on "resumedListening", (evt) ->
		  		updateStatus "resumed Listening"

			$(document).on "suspendRecognition", (evt) ->
		  		updateStatus "suspend Recognition"

			$(document).on "suspendedeRcognition", (evt) ->
		  		updateStatus "suspended Recognition"

			$(document).on "resumeRecognition", (evt) ->
		 	 	updateStatus "resume Recognition"

			$(document).on "resumedRecognition", (evt) ->
		  		updateStatus "resumed Recognition"

			$(document).on "startListeningWithLanguageModelAtPath", (evt) ->
				updateStatus "start Listening With Language Model At Path"
				detail = evt.originalEvent.detail
				updateStatus detail

			$(document).on "changeLanguageModelToFile", (evt) ->
				updateStatus "change Language Model To File"
				detail = evt.originalEvent.detail
				updateStatus detail				

			$(document).on "changedLanguageModelToFile", (evt) ->
				updateStatus "changed Language Model To File"
				detail = evt.originalEvent.detail
				updateStatus detail
				
			$(document).on "startedCalibration", (evt) ->
		  		updateStatus "started Calibration"

			$(document).on "finishedCalibration", (evt) ->
		  		updateStatus "finished Calibration"

			$(document).on "continuousSetupDidFail", (evt) ->
		  		updateStatus "continuous Setup Did Fail"

			$(document).on "testRecognitionCompleted", (evt) ->
		  		updateStatus "test Recognition Completed"


Flite

			$(document).on "say", (evt) ->
				updateStatus "say"
				detail = evt.originalEvent.detail
				updateStatus detail
				
			$(document).on "startedSpeaking", (evt) ->
				updateStatus "started Speaking"

			$(document).on "finishedSpeaking", (evt) ->
				updateStatus "finished Speaking"

			$(document).on "detectedSpeech", (evt) ->
		  		updateStatus "detected Speech"

			$(document).on "finishedDetectingSpeech", (evt) ->
		  		updateStatus "finished Detecting Speech"


				
Audio Session Listeners:
				
				
			$(document).on "audioSessionInterruptionDidBegin", (evt) ->
		  		updateStatus "audio Session Interruption Did Begin"

			$(document).on "audioSessionInterruptionDidEnd", (evt) ->
		  		updateStatus "audio Session Interruption Did End"

			$(document).on "audioInputDidBecomeUnavailable", (evt) ->
		  		updateStatus "audio Input Did Become Unavailable"

			$(document).on "audioInputDidBecomeAvailable", (evt) ->
		  		updateStatus "audio Input Did Become Available"

			$(document).on "audioRouteDidChangeToRoute", (evt) ->
		  		updateStatus "audio Route Did Change To Route"

