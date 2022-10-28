---
# type: posts 
title: "合并不同版本的Eclipse"
date: 2014-06-12T10:08:58+0800
authors: ["Jianan"]
summary: "一直以来都是用Eclipse IDE for Java and DSL Developers版本，这个版本没有集成J2EE的一些内容。官方提供另一个针对J2EE的版本，Eclipse IDE for J2EE Developers。想要使用服务器等Web的一些内容就需要使用J2EE的版本，或者自己安装J2EE的一些插件，比如很出名但要收费的MyEclipse插件。MyEclipse插件很强大，功能"
series: ["IDE"]
categories: ["IDE"]
tags: ["eclipse", "合并", "merge", "plugins"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2149
comment_num: 0
---

一直以来都是用Eclipse IDE for Java and DSL
Developers版本，这个版本没有集成J2EE的一些内容。官方提供另一个针对J2EE的版本，Eclipse IDE for J2EE
Developers。想要使用服务器等Web的一些内容就需要使用J2EE的版本，或者自己安装J2EE的一些插件，比如很出名但要收费的MyEclipse插件。MyEclipse插件很强大，功能很齐全，但是也会导致启动速度慢（也可能是我电脑硬件比较低的问题），所以不太想用MyEclipse。找了其它开源J2EE插件，很多都是直接绑定在Eclipse
IDE中以完整版提供下载。我想继续用我一直在使用的Eclipse IDE for Java and DSL Developers版本，但又想用Eclipse
IDE for J2EE
Developers版本的功能，于是想能不能直接将这两个版本合并！基于内核和插件模式的Eclipse理论上应该是行得通的，网上查了很多资料（这方面资料很少），最后看到这网页比较全，而且方法试验后证明是对的，现在我的Eclipse集成了两个Eclipse安装包，妥妥的！

  
原文链接：<http://stackoverflow.com/questions/6251063/how-to-combine-all-the-
packages-of-the-eclipse>  
  

原文内容：

How to merge several setup packages of Eclipse?

如何合并多个Eclipse安装包？

  

Whom this is for?

The ones that can not hold back their temper any longer when trying to install
another Eclipse function package. Right the installation speed from some
countries is too slow to bear, also the Equinox P2 always try to connect the
Download.Eclipse.org. Much to damn.. when your Internet connection closed or
reset all of a sudden and got all downloaded files broken. Also OSGi based
Eclipse plugins is chaos -- always have some conflict errors.

Oh that is another problem..

此文谨献给那些想要在现有的Eclipse中安装其他Eclipse包，但是却被某些国家的Eclipse更新体验逼疯的人。

  

Do the following steps:

方法就是以下几步：

1 Download the Install Packages that you need from [
www.eclipse.org](http://www.eclipse.org). Also please remember verify to see
if that is correct package. I choosed J2EE with C/C++.

首先下载从Eclipse的下载官网想要安装的安装包，记得检查下包是否完整。我这里选择了J2EE和C/C++。

  

2 Unpack one package with uncompress softwares, such as 7-zip and WinRAR.
unzip all the files to a directory you wanna install to. for example
"D:\Eclipse\".

将其中一个包用压缩软件，比如7-zip和WinRAR，解压到要安装位置。

  

3 open Configurations. Files
"\configuration\org.eclipse.equinox.source\source.info" and
"\configuration\org.eclipse.equinox.simpleconfigurator\bundles.info" in the
installation directory is the ones that need to be merged. also you need
"\configuration\org.eclipse.update\platform.xml".

打开所有安装包的文件，从所有的安装包寻找"\configuration\org.eclipse.equinox.source\source.info"、"\configuration\org.eclipse.equinox.simpleconfigurator\bundles.info"、"\configuration\org.eclipse.update\platform.xml"三个文件。

  

4 open the other packages and fetch their configuration files, and merge
files.

将这些文件复制到某个地方开始合并。

to Bundles.info:

Open the bundle.info with any Functional Text Editor, copy all text begin from
the line after "#version={Number}" and paste it to another. [{Number} means
any Integer number.]

Bundles.info文件：

使用稍微高级点的编辑器[别用Word之类的]，将里面所有从"#version={Number}"的下一行开始的文本全部复制到某一个里面[某一个是说其中一个你拿来做最后结果的文件]。[这里{Number}是说任何数字。]

to Source.info:

Similiar with what you did with the bundle.info. If not modified, that line
should be the 3rd line in the text file.

Source.info文件：

跟上一个的方法差不了多少。如果没修改过的话，应该是从第三行开始。

to Platform.xml:

Open the file, then find where "<feature id=" starts just after the "<site>"
node. Find the "</site>" tag and copy all the text between them, paste to
another file just before the "</site>" tag.

[You can do the similiar also marked <feature> tags with artifacts.xml]

Platform.xml文件：

打开，寻找在<site..>标签后第一个<feature id = 一直到</site>，全部复制，贴到另一个里面。

如果你需要处理artifacts.xml，也是这么一个办法。

  

5 when merging i suggest you to make a new directory and when finished please
remember copy the merged file to the one where should it be and overwrite. [I
mean where it comes from]

Although not merge "platform.xml" will not cause any functional errors, it
will make the About dialog with no button -- to ugly yeah?

[= =|||]Ugly is caused by the artifacts.xml in the installation directory...

我建议你在进行合并时候先准备一个文件夹存放那大堆的文件，然后合并完成之后再将合成的结果覆盖到原来的位置[这里指的是从ZIP包里面获得哪些文件的地方。]

虽然platform.xml不合并也不会造成功能性问题，但是About窗体可能会因此变得很丑。

[囧]发现其实是安装目录下的artifacts.xml导致的这个问题。

  

6 Open all the package, find "plugins" and "features" directories.now extract!

first extract the one you want most. I extracted JEE package.

then extract the other ones. I don't see any problem whether Overwrite the
ones or not.

现在打开所有的安装包，找到里面的plugins和features目录，全部解压到安装位置的对应地方。

比方说，我就先解压J2EE的包，然后才是其他的包。

其实，如果你下载的都是同时候出的，比方，全是3.6M2，那么覆盖不覆盖看你自己心情吧。

  

7 do open a console window, and locale in the installation directory, and then
execute "eclipse.exe".

最后打开一个命令行，在安装目录输入eclipse.exe然后执行。

Well, somebody ask me why i paid no attention to the Artifacts.xml in the
installation directory. That's because Eclipse will never check the file. It
seems to be when you want to update using zip files -- however this way is
blocked for lacking site.xml file now.

好吧，有人问我为什么不理会安装目录下的某个artifacts.xml……这个文件Eclipse根本就不管……实际上我就算是删了那文件，该打开的IDE还是照开不误……本来是为了之前版本的使用安装包更新，但是由于现在平台的Equinox更新，导致site.xml缺失——结果是这个方法不能用了。

Then guys, wait a several minutes for eclipse to do some sorting and cleaning
job for the merged configurations.. and install your plugins!

好吧，我记得刚搞完这段花了不少时间。还得等待几分钟才能看到第一次Eclipse的打开，因为Eclipse不得不处理下我们之前合并过的设置文件。等他打开，你就安装插件吧！

INFO: if you face some problems after install a new plugin and restart.. that
is because you haven't finish a complete artifacts.xml merging.

You may delete all the artifacts.xml 's header [document start to the
<artifacts size = '{Number}'>] and its bottom [from </artifacts> to document
end]. just merge the left content. and make one file just contains the header
and bottom, paste the merged one in.

Eh..maybe you can calculate the {Number}s' sum and correct the one in the
final document.

——————————————————————————

总结方法：主要是四个参数文件：Bundles.info、Source.info、Platform.xml、artifacts.xml，把不同安装包版本的这4个文件合并最后最后合并版本的参数文件。其中Bundles.info、Source.info两个文件同一个版本号的不同安装包中都包含了一些相同的参数，直接合并在一起也不会有太大的问题，但是我有代码洁癖。。所以写了几行代码来合并（原理是以行为单位比较筛选），源代码如下：

    
    
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.File;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.InputStreamReader;
    import java.util.Vector;
     
    public class CleanFiles {
    
    	private static Vector<String> stringlist;
    
    	public static void main(String[] args) throws Exception {
    		String filePath1;
    		String filePath2;
    		String filePath3;
    		String item;
    		stringlist = new Vector<String>();
    		BufferedReader buffer = new BufferedReader(new InputStreamReader(System.in));
    		System.out.println("请输入文件1路径和文件名：");
    		filePath1 = buffer.readLine();
    		System.out.println("请输入文件2路径和文件名：");
    		filePath2 = buffer.readLine();
    		System.out.println("请输入合并后文件路径和文件名");
    		filePath3 = buffer.readLine();
    		File file1 = new File(filePath1);
    		File file2 = new File(filePath2);
    		File file3 = new File(filePath3);
    		buffer.close();
    		buffer = new BufferedReader(new FileReader(file1));
    		while((item=buffer.readLine())!=null){
    			 addString(stringlist,item);
    		}
    		buffer.close();
    		buffer = new BufferedReader(new FileReader(file2));
    		while((item= buffer.readLine())!=null){
    			addString(stringlist,item);
    		}
    		buffer.close();
    	 
    		// 搜索结果是经过排序的，根据此规律删除不合要求File
    		for (int i = 0; i < stringlist.size()-1; i++) {
    			if(stringlist.get(i).equals(stringlist.get(i+1))){
    				stringlist.remove(i);
    				if(i!=0)i--;
    			}
    		}
    	   
    		BufferedWriter bufferWriter = new BufferedWriter(new FileWriter(file3));
    			for(String temp:stringlist){
    				bufferWriter.write(temp);
    				bufferWriter.newLine();
    			}
    			bufferWriter.close();
    		}
    	
    		public static void addString(Vector<String> allStrings, String str) {
    		if (allStrings.isEmpty()) {
    			allStrings.add(str);
    		} else {
    		
    			// 二分查找法
    			int left = 0, right = allStrings.size() - 1, middle, compare;
    			if (str.compareToIgnoreCase(allStrings.get(right)) >= 0) {
    				allStrings.add(str);
    				return;
    			}
    			if (str.compareToIgnoreCase(allStrings.get(0)) <= 0) {
    				allStrings.add(0, str);
    				return;
    			}
    			while (true) {
    				middle = (left + right) / 2;
    				compare = str.compareToIgnoreCase(allStrings.get(middle));
    				if (compare == 0 || middle == left) {
    					allStrings.add(middle + 1, str);
    					return;
    				} else {
    					if (compare < 0) {
    						right = middle;
    					} else {
    						left = middle;
    					}
    				}
    			}
    		}
    	}
    }

  

Platform.xml文件合并后，也有很多重复的节点，写了另一段代码来处理（当然不处理应该也是没问题的），原理是标记每一行节点的顺序，再进行筛选，最后再按原顺序排序，同样直接把代码贴这里（备注：我用这段代码处理的时候不知道为什么最后第二个</site>会被删除，研究很久没想明白，最晚弄太晚就没弄，自己手工加上去）：

    
    
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.File;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.InputStreamReader;
    import java.util.Vector;
    
    public class CleanXMLFiles {
    
            private static Vector<XMLElement> xmllist;
    
            public static void main(String[] args) throws Exception {
                    String filePath1;
                    String filePath2;
                    String element;
                    String item;
                    xmllist = new Vector<XMLElement>();
                    BufferedReader buffer = new BufferedReader(new InputStreamReader(
                                    System.in));
                    System.out.println("请输入xml文件路径和文件名：");
                    filePath1 = buffer.readLine();
                    System.out.println("请输入清理后文件路径和文件名");
                    filePath2 = buffer.readLine();
                    System.out.println("请输入清理的节点名称：");
                    element = buffer.readLine();
                    File file1 = new File(filePath1);
                    File file2 = new File(filePath2);
                    buffer.close();
                    buffer = new BufferedReader(new FileReader(file1));
                    int fileId = 0;
                    while ((item = buffer.readLine()) != null) {
                            StringBuffer stringBuffer = new StringBuffer();
                            stringBuffer.append(item);
                            if (item.trim().startsWith("<" + element)) {
                                    do {
                                            if((item = buffer.readLine())!=null){
                                                    stringBuffer.append(item);
                                            }
                                    } while (!(item.trim().endsWith("</" + element + ">")));
    
                            }
                            addString(xmllist,new XMLElement(fileId++,stringBuffer.toString()));
                    }
                    buffer.close();
    
                    // 搜索结果是经过排序的，根据此规律删除不合要求File
                    for (int i = 0; i < xmllist.size() - 1; i++) {
                            if (xmllist.get(i).getElementString().trim().equals(xmllist.get(i + 1).getElementString().trim())) {
                                    xmllist.remove(i);
                            }
                    }
                    
                    XMLElement big;
                    for(int i=0;i<xmllist.size();i++){
                            for(int j=i+1;j<xmllist.size();j++){
                                    if(xmllist.get(i).getId()>xmllist.get(j).getId()){
                                            big = xmllist.get(i);
                                            xmllist.set(i, xmllist.get(j));
                                            xmllist.set(j, big);
                                    }
                            }
                    }
                    
    
                    BufferedWriter bufferWriter = new BufferedWriter(new FileWriter(file2));
                    for (XMLElement temp : xmllist) {
                            bufferWriter.write(temp.getElementString());
                            bufferWriter.newLine();
                    }
                    bufferWriter.close();
    
            }
    
            public static void addString(Vector<XMLElement> allStrings,
                            XMLElement elementTemp) {
                    if (allStrings.isEmpty()) {
                            allStrings.add(elementTemp);
                    } else {
    
                            // 二分查找法
                            int left = 0, right = allStrings.size() - 1, middle, compare;
                            if (elementTemp.getElementString().compareToIgnoreCase(
                                            allStrings.get(right).getElementString()) >= 0) {
                                    allStrings.add(elementTemp);
                                    return;
                            }
                            if (elementTemp.getElementString().compareToIgnoreCase(
                                            allStrings.get(0).getElementString()) <= 0) {
                                    allStrings.add(0, elementTemp);
                                    return;
                            }
                            while (true) {
                                    middle = (left + right) / 2;
                                    compare = elementTemp.getElementString().compareToIgnoreCase(
                                                    allStrings.get(middle).getElementString());
                                    if (compare == 0 || middle == left) {
                                            allStrings.add(middle + 1, elementTemp);
                                            return;
                                    } else {
                                            if (compare < 0) {
                                                    right = middle;
                                            } else {
                                                    left = middle;
                                            }
                                    }
                            }
                    }
            }
    }
    
    class XMLElement {
            private int id;
            private String elementString;
    
            public XMLElement(int id, String elementString) {
                    super();
                    this.id = id;
                    this.elementString = elementString;
            }
    
            public int getId() {
                    return id;
            }
    
            public void setId(int id) {
                    this.id = id;
            }
    
            public String getElementString() {
                    return elementString;
            }
    
            public void setElementString(String elementString) {
                    this.elementString = elementString;
            }
    }

  
至于artifacts.xml文件，我查的时候貌似没有多少重复的节点，就没有处理。  
  
以上折腾纯属个人手贱，仅供有需要人士参考。  

