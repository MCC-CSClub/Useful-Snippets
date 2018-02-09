## Simple TTS
A simple TTS class which uses Talkify.net's TTS service to play a given input. Test out the service today http://talkify.net/

### Dependencies
Unforunately this also requires mp3plugin.jar to be added to your class path in order for mp3 decoding to work properly.
mp3plugin.jar is an old library that's hard to find on the internet now adays. I've provided a link to one from my website
[here](http://michaelwflaherty.com/files/mp3plugin.jar). Enjoy


### Usage
Simple usage looks like `TextToSpeech.say("Hello!");`


```java
/*  Simple Text-To-Speech Class - I gave myself a headache so you don't have to.
 *
 *  Copyright (C) 2017 Michael Flaherty
 *
 * This program is free software: you can redistribute it and/or modify it
 * under the terms of the GNU General Public License as published by the Free
 * Software Foundation, either version 3 of the License, or (at your option)
 * any later version.
 *
 * This program is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
 * FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License along with
 * this program. If not, see http://www.gnu.org/licenses/.
 */

import java.net.URL;

import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.AudioFormat;
import javax.sound.sampled.SourceDataLine;
import javax.sound.sampled.DataLine.Info;

public class TextToSpeech
{
    public static final String URL = "http://talkify.net/api/Speak?&text=";
    public static final String URL_PARAMS = "&format=mp3&refLang=0&id=63080db1-8da8-47ee-bde1-07bbce440a70&voice=&rate=0";

    // Description: Plays the TTS for a given input using talkify's servers.
    public static void say(String input) throws Exception
    {
        // Get Incoming audio stream
        AudioInputStream inputStream = AudioSystem.getAudioInputStream(new URL(TextToSpeech.formatURL(input)));

        // Decode
        AudioFormat original = inputStream.getFormat();
        AudioFormat decoded = new AudioFormat(AudioFormat.Encoding.PCM_SIGNED, original.getSampleRate(), 16, original.getChannels(), original.getChannels() * 2, original.getSampleRate(), false);

        AudioInputStream decodedInputStream = AudioSystem.getAudioInputStream(decoded, inputStream);
        Info info = new Info(SourceDataLine.class, decoded);

        // Get line
        SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);

        if(line != null)
        {
            line.open(decoded);
            line.start();

            int nBytesRead;
            byte[] data = new byte[4096];

            while ((nBytesRead = decodedInputStream.read(data, 0, data.length)) != -1)
            {
                line.write(data, 0, nBytesRead);
            }

            line.drain();
            line.stop();
            line.close();
        }

        decodedInputStream.close();
    }

    private static String formatURL(String input)
    {
        String[] temp;
        String formattedInput, output;

        formattedInput = "";
        temp = input.split(" ");

        for (int i = 0; i < temp.length - 1; i++)
        {
            formattedInput += temp[i] + "%20";
        }

        formattedInput += temp[temp.length-1];

        output = URL + formattedInput + URL_PARAMS;

        return output;
    }

    public static void main(String[] args)
    {
        try
        {
            TextToSpeech.say("Hello, My name is Michael. This is a test. Enjoy");
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }
}
```
