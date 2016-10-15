# CompilerPrinciple
this is a program of compilerprinciple
I make some change for the Algrith.
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.util.Scanner;

public class SyntaxAnalysis {
	static char proce[]=new char[800];//输入的程序段的保存
	static char token[]=new char[8];
	
	static char ch;
	static String reserve[]={"begin","if","then","while","do","end",};//保留字集合
	static int p;//iden为识别出的key，标识符等的识别码为整数
	static int sum;
	static int m;
	static int iden;
	static int row=0;
	static int flag=0;

	public static void main(String[] args) throws FileNotFoundException {
		// TODO Auto-generated method stub
		String filepath="input.txt";
		FileOutputStream bos = new FileOutputStream("output.txt");
		    p=0;
		    row=1;
		    String c= readTxtFile(filepath);//文本文件读取输入程序
		    proce=c.toCharArray();
		    p=0;
		    do
		    {
		        scan();
		        switch(iden)
		        {
		        case 11: 
		          System.out.println("("+iden+","+sum+")");
		          System.setOut(new PrintStream(bos));
		          break;
		        case -1: 
		          System.out.println("Error in row"+row+"!");
		          System.setOut(new PrintStream(bos));
		          break;
		        case -2: 
		        	row++;
		        	System.setOut(new PrintStream(bos));
				 
		        break;
		        
		        default:
		        	System.setOut(new PrintStream(bos));
		        	System.out.print("("+iden+",");
		        	for(int i=0;i<8;i++)
		        	{
		        		if(token[i]!=' ')
		        		 System.out.print(token[i]);
		        	}
		        	System.out.println(")");
		        	System.setOut(new PrintStream(bos));
		         break;
		        }
		    }
		    while (iden!=0);
		    System.out.println("Analysis Complished");
	}
	static void scan()//扫描输入的文本
	{
		/*
		 * 扫面程序段
		 */	
		for(int n=0;n<8;n++)
		{
			token[n]=' ';
		}
		ch=proce[p++];
		while(ch==' ')
		{
			ch=proce[p];
			p++;
		}
		if((ch>='a'&&ch<='z')||(ch>='A'&&ch<='Z'))//识别标识符关键字等
		{
			m=0;
			while((ch>='0'&&ch<='9')||(ch>='a'&&ch<='z')||(ch>='A'&&ch<='Z'))
			{
				token[m++]=ch;
				ch=proce[p++];
				//p++;
			}
			token[m++]='\0';
			p--;
			for(int i=flag;i<6;i++)
			{
				char lk[]=new char[reserve[i].length()];
				lk=reserve[i].toCharArray();
				char arr[]=new char[lk.length];
				for(int j=0;j<lk.length;j++)
				{
					arr[j]=token[j];
				}
				int carry=0;
			   for(int po=0;po<lk.length;po++)
			    {
				    if(arr[po]==lk[po])
				    {
					  carry++;
				    }
			    }
			   if(carry==lk.length)
			   {
					iden=i+1;
					flag++;
					break;
					
			   }
			   else
			   {
				 iden=10;
				 break;
			   }
			}
			
		}
		else if((ch>='0'&&ch<='9'))//识别数字
		{
			{
				sum=0;
				while(((ch>='0'&&ch<='9')))
				{
					sum=sum*10+ch-'0';
					ch=proce[p++];
				}
			}
			p--;
			iden=11;
			if(sum>32767)
				iden=-1;
		}
		else switch(ch)//识别运算符，界限符等
		{
		case '<':
			m=0;
			token[m++]=ch;
			if(ch=='>')
			{
				iden=21;
				token[m++]=ch;
			}
			else if(ch=='=')
	            {
	                iden=22;
	                token[m++]=ch;
	            }
	            else
	            {
	                iden=23;
	                p--;
	            }
	            break;
		
	 case'>':
		 m=0;token[m++]=ch;
         ch=proce[p++];
         if(ch=='=')
         {
             iden=24;
             token[m++]=ch;
         }
         else
         {
             iden=20;
             p--;
         }
         break;
     case':':
    	 m=0;token[m++]=ch;
         ch=proce[p++];
         if(ch=='=')
         {
             iden=18;
             token[m++]=ch;
         }
         else
         {
             iden=17;
             p--;
         }
         break;
     case'*':iden=13;token[0]=ch;break;
     case'/':iden=14;token[0]=ch;break;
     case'+':iden=15;token[0]=ch;break;
     case'-':iden=16;token[0]=ch;break;
     case',':iden=17;token[0]=ch;break;
     case'.':iden=18;token[0]=ch;break;
     case'=':iden=25;token[0]=ch;break;
     case';':iden=26;token[0]=ch;break;
     case'(':iden=27;token[0]=ch;break;
     case')':iden=28;token[0]=ch;break;
     case'#':iden=0;token[0]=ch;break;
     case'\n':iden=-2;break;
     default: iden=-1;break;
		
		}
	}
	public static String readTxtFile(String filePath){
		String opt="";
        try {
        	    String lineTxt;
                String encoding="GBK";
                File file=new File(filePath);
                if(file.isFile() && file.exists()){ //判断文件是否存在
                    InputStreamReader read = new InputStreamReader(
                    new FileInputStream(file),encoding);//考虑到编码格式
                    BufferedReader bufferedReader = new BufferedReader(read);
                    while((lineTxt = bufferedReader.readLine()) != null){
                         opt+=lineTxt;
                    }
                    read.close();
        }else{
            System.out.println("找不到指定的文件");
        }
        } catch (Exception e) {
            System.out.println("读取文件内容出错");
            e.printStackTrace();
        }
		return opt;//返回读取的txt文件
    }

}
