## Description
Creates a notification through the operating system's `SystemTray`. I have only tested this on a Windows 10 system, but should work cross platform. If the system tray is not found, an `AWTException` will be thrown.

I've included an overload if you'd like to pass an `ActionListener` into `sendNotification`. This is if you'd like to register whether or not the user has clicked on the notification.

## Code
```java
public static void sendNotification(String title, String message) throws AWTException
{
	sendNotification(title, message, null);
}

public static void sendNotification(String title, String message, ActionListener listener) throws AWTException
{
	SystemTray tray = SystemTray.getSystemTray();

	Image image = Toolkit.getDefaultToolkit().createImage("icon.png");
	TrayIcon icon = new TrayIcon(image, "Demo");
	icon.setImageAutoSize(true);
	icon.setToolTip(title);
	tray.add(icon);

	if (listener != null)
	{
		icon.addActionListener(listener);
	}

	icon.displayMessage(title, message, MessageType.INFO);
}
```