1)BRIDGE PATTERN 

DrawAPI.java

public interface DrawAPI 


{

   public void drawCircle(int radius, int x, int y);

}


RedCircle.java
public class RedCircle implements DrawAPI 

{
  
  @Override
   public void drawCircle(int radius, int x, int y) 
   
   {
   
      System.out.println("Drawing Circle[ color: red, radius: " + radius + ", x: " + x + ", " + y + "]");
  
  }
  
}



GreenCircle.java

public class GreenCircle implements DrawAPI 

{
  
  @Override
    public void drawCircle(int radius, int x, int y) 
  
  {
      
      System.out.println("Drawing Circle[ color: green, radius: " + radius + ", x: " + x + ", " + y + "]");
   
   }
   
}


Shape.java

public abstract class Shape 

{

   protected DrawAPI drawAPI;
   
   protected Shape(DrawAPI drawAPI)
   {
   
            this.drawAPI = drawAPI;
            
   }
   
   public abstract void draw();	
   
}


Circle.java

public class Circle extends Shape 

{

   private int x, y, radius;
   

   public Circle(int x, int y, int radius, DrawAPI drawAPI) 
   
   {
   
      super(drawAPI);
      this.x = x;  
      this.y = y;  
      this.radius = radius;
      
   }

   public void draw() 
   
   {
   
      drawAPI.drawCircle(radius,x,y);
      
   }
   
}


BridgePattern.java

public class BridgePattern 

{

   public static void main(String[] args) 
   
   {
   
      Shape redCircle = new Circle(100,100, 10, new RedCircle());
      Shape greenCircle = new Circle(100,100, 10, new GreenCircle());

      redCircle.draw();
      greenCircle.draw();
      
   }
   
}




2)ADAPTER PATTERN 

MediaPlayer.java

public interface MediaPlayer 

{

   public void play(String audioType, String fileName);
   
}


AdvancedMediaPlayer.java
public interface AdvancedMediaPlayer 

{	

   public void playVlc(String fileName);
   public void playMp4(String fileName);
   
}


VlcPlayer.java
public class VlcPlayer implements AdvancedMediaPlayer

{
   @Override
   public void playVlc(String fileName) 
   
   {
   
      System.out.println("Playing vlc file. Name: "+ fileName);	
      
   }

   @Override
   public void playMp4(String fileName) 
   {
   
   }
   
}



Mp4Player.java
public class Mp4Player implements AdvancedMediaPlayer
{

   @Override
   public void playVlc(String fileName) 
   {
      //do nothing
   }

   @Override
   public void playMp4(String fileName) 
   {
      System.out.println("Playing mp4 file. Name: "+ fileName);		
   }
}


MediaAdapter.java
public class MediaAdapter implements MediaPlayer 
{

   AdvancedMediaPlayer advancedMusicPlayer;

   public MediaAdapter(String audioType)
   {
   
      if(audioType.equalsIgnoreCase("vlc") )
      {
         advancedMusicPlayer = new VlcPlayer();			
         
      }else if (audioType.equalsIgnoreCase("mp4"))
      {
         advancedMusicPlayer = new Mp4Player();
      }	
   }

   @Override
   public void play(String audioType, String fileName) 
   {
   
      if(audioType.equalsIgnoreCase("vlc"))
      {
         advancedMusicPlayer.playVlc(fileName);
      }
      else if(audioType.equalsIgnoreCase("mp4"))
      {
         advancedMusicPlayer.playMp4(fileName);
      }
   }
}


AudioPlayer.java
public class AudioPlayer implements MediaPlayer 
{
   MediaAdapter mediaAdapter; 

   @Override
   public void play(String audioType, String fileName) 
   {		

      //inbuilt support to play mp3 music files
      if(audioType.equalsIgnoreCase("mp3"))
      {
         System.out.println("Playing mp3 file. Name: " + fileName);			
      } 
      
      //mediaAdapter is providing support to play other file formats
      else if(audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4"))
      {
         mediaAdapter = new MediaAdapter(audioType);
         mediaAdapter.play(audioType, fileName);
      }
      
      else{
         System.out.println("Invalid media. " + audioType + " format not supported");
      }
   }   
}


AdapterPattern.java
public class AdapterPattern{
   public static void main(String[] args) 
   {
      AudioPlayer audioPlayer = new AudioPlayer();

      audioPlayer.play("mp3", "beyond the horizon.mp3");
      audioPlayer.play("mp4", "alone.mp4");
      audioPlayer.play("vlc", "far far away.vlc");
      audioPlayer.play("avi", "mind me.avi");
   }
}


3)FACADE PATTERN 
MobileShop.java
public interface MobileShop
{  
    public void modelNo();  
    public void price();  
}  


Iphone.java
public class Iphone implements MobileShop 
{  
    @Override  
    public void modelNo() 
    {  
        System.out.println(" Iphone 13 ");  
    }  
    @Override  
   
    public void price() 
    {  
    System.out.println(" Tg 500.000 ");  
    }  
}  


Samsung.java
public class Samsung implements MobileShop
{  
    @Override  
    public void modelNo() 
    {  
    System.out.println(" Samsung galaxy 12 ");  
    }  
    @Override  
    public void price() 
    {  
        System.out.println(" Tg 375.000 ");  
    }  
}  


Nokia.java
public class Nokia implements MobileShop 
{  
    @Override  
    public void modelNo() 
    {  
    System.out.println(" Nokia Z10 ");  
    }  
    @Override  
    public void price() 
    {  
        System.out.println(" Tg 200.000 ");  
    }  
}  


ShopKeeper.java
public class ShopKeeper 
{  
    private MobileShop iphone;  
    private MobileShop samsung;  
    private MobileShop nokia;  
      
    public ShopKeeper()
    {  
        iphone= new Iphone();  
        samsung=new Samsung();  
        blackberry=new Nokia();  
    }  
    public void iphoneSale()
    {  
        iphone.modelNo();  
        iphone.price();  
    }  
        public void samsungSale()
        {  
        samsung.modelNo();  
        samsung.price();  
    }  
   public void NokiaSale()
   {  
    blackberry.modelNo();  
    blackberry.price();  
        }  
}  


Facade.java
public class FacadePatternClient 
{  
    private static int  choice;  
    public static void main(String args[]) throws NumberFormatException, IOException
    {  
        do
        {       
            System.out.print("========= Mobile Shop ============ \n");  
            System.out.print("            1. IPHONE.              \n");  
            System.out.print("            2. SAMSUNG.              \n");  
            System.out.print("            3. NOKIA.            \n");  
            System.out.print("            4. Exit.                     \n");  
            System.out.print("Enter your choice: ");  
              
            BufferedReader br=new BufferedReader(new InputStreamReader(System.in));  
            choice=Integer.parseInt(br.readLine());  
            ShopKeeper sk=new ShopKeeper();  
              
            switch (choice) 
            {  
            case 1:  
                {   
                  sk.iphoneSale();  
                    }  
                break;  
       case 2:  
                {  
                  sk.samsungSale();        
                    }  
                break;    
       case 3:  
                           {  
                           sk.nokiaSale();       
                           }  
                   break;     
            default:  
            {    
                System.out.println("Nothing You purchased");  
            }         
                return;  
            }  
              
      }while(choice!=4);  
   }  
}  
