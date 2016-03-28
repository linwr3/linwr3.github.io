---
layout: post
comments: false
categories: RFID
---

It's the beginning of learning RFID. It need to install these software first:
* [Eclipse](http://www.eclipse.org/downloads/packages/release/Juno/SR2)
* [JCOP Tool](http://download.csdn.net/download/cctv5080/4684165)(java card open platform)

It should be noted that if your jdk is 32-bit, then your eclipse must be 32-bit. For example, my eclipse is ==eclipse-cpp-juno-win32.zip== and jdk is ==jdk-8u74-windows-i586==.

If you have installed 64-bit jdk, but the eclipse is 32-bit. In this case, you can install another 32-bit jdk and add this code at the beginning of eclipse.ini:

```
-vm
C:/Program Files (x86)/Java/jre1.8.0_74/bin/javaw.exe
```

Then you can keep the environment variable for 64-bit jdk.

***
Now, let's install JCOP.

##### Step one: Installation

Start eclipse and click ==Help==, ==Install New Software==, ==Add==, ==archive==. And select the ==JCOP Toll==, click on ==OK== button.
Select ==Uncategorized== and click ==Next== to start.
Click ==OK== if the warning pop up. Select ==JCOP== if the ==Selection Needed== pop up.

##### Step two: Activation

Quit eclipse. New a file named ==com.ibm.bluez.jcop.eclipse.prefs== under your workspace `.\metadata\.plugins\org.eclipse.core.runtime\.settings`, and write down:

```
com.ibm.bluez.jcop.eclipse.views.bytecode.weights.1=333
com.ibm.bluez.jcop.eclipse.views.bytecode.weights.0=666
eclipse.preferences.version=1
com.ibm.bluez.jcop.eclipse.views.shell.trace=true
com.ibm.bluez.jcop.eclipse.token=23cb832f9bc9c8bffe21d53e8f02e5bc
```

##### Step three: Hello World

Start eclipse. Click ==File==, ==New==, ==Others==, ==Java Card==, ==Java Card Project==, then named the first project.
Right-click the project, and click ==Java Card Applet==, then create a package and a java class. Click ==Next== then set the package AID and applet AID, ==Finish==.

My first applet is to send "hello" when selected.

```
public class Hello extends Applet {
	public static void install(byte[] bArray, short bOffset, byte bLength) {
		// GP-compliant JavaCard applet registration
		new Hello().register(bArray, (short) (bOffset + 1), bArray[bOffset]);
	}
	public void process(APDU apdu) {
		// Good practice: Return 9000 on SELECT
		byte ch[]={'h','e','l','l','o'};
		byte[] buf = apdu.getBuffer();
		if (selectingApplet()) {
        	// to send the data received
			//apdu.setOutgoingAndSend((short)5, apdu.setIncomingAndReceive());

			Util.arrayCopyNonAtomic(ch, (short)0, buf, (short)0, (short)ch.length);
			apdu.setOutgoingAndSend((short)0, (short)ch.length);
			return;
		}
		switch (buf[ISO7816.OFFSET_INS]) {
		case (byte) 0x00:
			break;
		default:
			// good practice: If you don't know the INStruction, say so:
			ISOException.throwIt(ISO7816.SW_INS_NOT_SUPPORTED);
		}
	}
}
```

Click ==Run Configurations== and right-click ==Java Card Application==, ==new==, ==Run==. Type `/select 112233445566`(select your applet AID).

If your eclipse and jdk is 64-bit, then maybe it will pop up ==Problem Occurred== and the details like ==..\eclipse\plugins\com.ibm.bluez.jcop.eclipse_3.1.2\simuls\nJCOP\win32\x86_64\jcop.exe could not found== when running the program. The solution is to rename the file folder ==x86== as ==x86_64==.

Well, my first applet works smoothly.
