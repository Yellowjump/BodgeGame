# BodgeGame
//a easy game
import javax.swing.*;
import java.awt.*;
import java.lang.*;
import java.awt.event.*;
class MyJFrame extends JFrame implements Runnable{
		Dimension screen=Toolkit.getDefaultToolkit().getScreenSize();
		int width=screen.width;    //屏幕宽度，以像素为单位
		int height=screen.height;    //屏幕高度
		double positionx=0;
		double positiony=0;
		double positionmanx=195;
		double positionmany=160;
		double julix=5;
		double juliy=3;
		long waittime=50;
		int Score=0;
		JFrame window=new JFrame("贪吃蛇");
		JButton StartGame=new JButton("开始游戏");
		JButton Exit=new JButton("退出");
		JPanel Begin=new JPanel();
		ImageIcon picture=new ImageIcon("dazui.jpg");
		JLabel pictureLabel=new JLabel(picture);
		ImageIcon man=new ImageIcon("benpao.png");
		JLabel pictureman=new JLabel(man);
		JPanel gaming=new JPanel();
		Thread thread1,thread2;//两个线程
		public MyJFrame(){
			window.setSize(500, 500);
			window.setLocation((int)positionx,(int)positiony);
			
			FlowLayout f=new FlowLayout(FlowLayout.CENTER,200,200);
			Begin.setLayout(f);
			Begin.setBackground(new Color(80,90,80));
			Begin.add(StartGame); 
			Begin.add(Exit);
			gaming.setLayout(null);
			gaming.add(pictureman);
			gaming.add(pictureLabel);
			pictureLabel.setSize(500, 500);
			pictureLabel.setLocation(0, -20);
			pictureman.setSize(100, 100);
			pictureman.setLocation((int)positionmanx,(int)positionmany);
			window.getContentPane().add(Begin);
			window.setVisible(true);
			window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			StartGame.addActionListener(new ActionListener(){
				public void actionPerformed(ActionEvent e){
					MyJFrame.this.start();//这个方法可以得到这个对象
				}
			});
			Exit.addActionListener(new ActionListener(){
				public void actionPerformed(ActionEvent e){
					System.exit(0);
				}
			});
			
		}
		public void start(){
			thread1=new Thread(this,"Move");
			thread2=new Thread(this,"isexit");
			thread1.start();
			thread2.start();
		}
		public void run(){//Runnable的run方法
			if(Thread.currentThread()==thread2){
			window.addMouseMotionListener(new MouseMotionListener(){
				public void mouseMoved(MouseEvent m){
					positionmanx=m.getX()-55;
					positionmany=m.getY()-90;
					pictureman.setLocation((int)positionmanx,(int)positionmany);
					if(positionmany<=20||positionmanx<=50||positionmanx>=350||positionmany>=332)
						{
						JFrame gameOver=new JFrame("游戏结束");
						gameOver.setVisible(true);
						window.dispose();
						gameOver.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
						gameOver.setSize(500, 500);
						gameOver.setLocation((int)positionx,(int)positiony);
						JPanel reGame=new JPanel();
						reGame.setLayout(new FlowLayout(FlowLayout.CENTER,200,100));
						JButton Return=new JButton("重新游戏");
						JLabel score=new JLabel("你的得分为"+String.valueOf(Score));
						
						reGame.add(score);
						reGame.add(Return);
						gameOver.add(reGame);
						Return.addActionListener(new ActionListener(){
							public void actionPerformed(ActionEvent e){
								MyJFrame mj=new MyJFrame();
								gameOver.dispose();
								
							}
						});
						}
					//System.out.println("X="+m.getX()+"Y="+m.getY());
					
				}
				public void mouseDragged(MouseEvent m){
					window.dispose();
				}
			});}
			else if(Thread.currentThread()==thread1){
				Begin.setVisible(false);
				window.add(gaming);
				
				/*try {
					thread2.sleep((long)2000);
				} catch (InterruptedException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}*/
				while(true){Score++;
					try {
						Thread.sleep(10);
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					if(positionx+julix>0&&positionx+julix<(width-500)&&positiony+juliy>0&&positiony+juliy<(height-500)){
						window.setLocation((int)(positionx+=julix),(int)( positiony+=juliy));
					}
					else if(positionx+julix<=0||positionx+julix>=(width-500)){
						julix=(5f+Math.random()*10f)*(-julix/Math.abs(julix));
						//System.out.println(positionx+"+"+positiony);
						//System.out.println("julix="+julix+"juliy="+juliy);
				
					}
					else {
						juliy=(3f+(Math.random()*6f))*(-juliy/Math.abs(juliy));
						//System.out.println(positionx+"+"+positiony);
						//System.out.println("julix="+julix+"juliy="+juliy);
					}
				}
			}
		}
		
}
public class DodgeGame {
	public static void main(String[] args){
		MyJFrame mj=new MyJFrame();
		
		
	}
}
