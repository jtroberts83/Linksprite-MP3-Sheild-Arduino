# Linksprite-MP3-Sheild-Arduino
All the files to use Linksprite MP3 shield with Arduino Uno

Copy all the files in this repo to your My Documents\Arduino\Music folder 
Then open up the file called music.ino within the folder.  
You will see multiple tabs on the top, those are all the different pieces of the program.  
You will see one tab called player.cpp that’s where most of the code is that you might want to edit.  
 
Click on the player.cpp tab in the program and then scroll down or search for the following line:
 
unsigned char g_volume = 40;//used for controling the volume
 
That is the code that sets the default volume.  
So you can change 40  to any number between 0 (loudest) - 254 (turned down all the way)
 
Then below that code you will find the following function that checks for key presses.  
Below the PSKey is Play/Pause, NTKey is Next Track, BKKey is Back Track, VUKey is Volume Up, and VDKey is Volume Down key:
 
void CheckKey()
{
  //static unsigned char volume = 40;
  static unsigned int vu_cnt = 1000;//volume up interval
  static unsigned int vd_cnt = 1000;//volume down interval
 
  
  if(0 == PSKey)
  {
       playStop = 1-playStop;
       delay(20);
       while(0 == PSKey);
       delay(20);
 
  }
  
 
  if(0 == NTKey)
  {
       playingState = PS_NEXT_SONG;
       delay(20);
       while(0 == NTKey);
       delay(20);
  }
  else if(0 == BKKey)
  {
    playingState = PS_PREVIOUS_SONG;
       delay(20);
       while(0 == BKKey);
       delay(20);
  }
  else if(0 == VUKey)
  {
       if(--vu_cnt == 0)
       {
              if (g_volume-- == 0) g_volume = 0; //Change + limit to 0 (maximum volume)
    
              Mp3SetVolume(g_volume,g_volume);         
 
              redPwm = (175-g_volume)*3>>1;
              if(redPwm >255)
              {
                     redPwm = 255;
              }
              if(redPwm < 0)
              {
                     redPwm = 0;
              }
              
              //Serial.println(redPwm,DEC);
              vu_cnt = 1000;
       }
  }
  else if (0 == VDKey)
  {
    if(--vd_cnt == 0)
       {
              if (g_volume++ == 254) g_volume = 254; //Change + limit to 254 (minimum vol)
       
             Mp3SetVolume(g_volume,g_volume);
 
              redPwm = 305-(g_volume<<1);
              if(redPwm >255)
              {
                     redPwm = 255;
              }
              if(redPwm < 0)
              {
                     redPwm = 0;
              }
              //Serial.println(redPwm,DEC);
              vd_cnt = 1000;
       }
                       
  }
  
  
}

