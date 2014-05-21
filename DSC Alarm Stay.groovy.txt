/**
 *  DSC Alarm
 *
 *  Author: ethomasii@gmail.com
 *  Date: 2013-12-12
 */
 // for the UI

preferences {
    input("host", "text", title: "URL", description: "The URL of your alarmserver")
    input("port", "text", title: "Port", description: "The port")
} 
 
metadata {
	// Automatically generated. Make future change here.
	definition (name: "DSC Alarm Stay", author: "ethomasii@gmail.com") {
		capability "Alarm"
		capability "Switch"

		command "arm"
		command "disarm"
		command "stay"
		command "disarm2"
	}

	// simulator metadata
	simulator {
	}

	// UI tile definitions
	tiles {
		standardTile("button", "device.switch", width: 1, height: 1, canChangeIcon: true) {
			state "off", label: 'Stay', action: "switch.on", icon: "st.Home.home4", backgroundColor: "#ccffcc"
			state "on", label: 'Disarm', action: "switch.off", icon: "st.Home.home4", backgroundColor: "#EE0000"

}

		main "button"
		details(["button"])
	}
}

def parse(String description) {
}

def sendCmd(apiCall)
{
	httpGet("http://${settings.host}:${settings.port}/${apiCall}") {response -> 
        def content = response.data
        log.debug content
    }
}

def arm() {
	//sendCmd("api/alarm/stayarm")
	//sendEvent(name: "button", value: "arm")
	log.debug "Executing 'arm'"
	// TODO: handle 'strobe' command
	sendEvent(name: "switch", value: "on");            
}

def on() {
    //right now do not arm or disarm... 
    sendCmd("api/alarm/stayarm")
	//sendEvent(name: "button2", value: "stay")
    log.debug "Executing 'stay'"
	// TODO: handle 'siren' command
	sendEvent(name: "switch", value: "on");        
}

def disarm() {
	//right now do not arm or disarm... 
    //sendCmd("api/alarm/disarm")
	//sendEvent(name: "button", value: "disarm")
	log.debug "Executing 'disarm'"
	// TODO: handle 'both' command
	sendEvent(name: "switch", value: "off");    
}

def off() {
	//right now do not arm or disarm... 
    sendCmd("api/alarm/disarm")
	//sendEvent(name: "button2", value: "disarm2")
	log.debug "Executing 'disarm'"
	// TODO: handle 'both' command
	sendEvent(name: "switch", value: "off");        
}



