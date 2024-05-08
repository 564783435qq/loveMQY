1、阿里中央仓库（首选推荐）

<repository> 
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</repository>
2、camunda.com 中央仓库（第2推荐使用）

<repository> 
    <id>activiti-repos2</id> 
    <name>Activiti Repository 2</name> 
    <url>https://app.camunda.com/nexus/content/groups/public</url> 
</repository> 
3、spring.io 中央仓库

<repository> 
    <id>springsource-repos</id> 
    <name>SpringSource Repository</name> 
    <url>http://repo.spring.io/release/</url> 
</repository>
4、maven.apache.org 中央仓库

<repository> 
    <id>central-repos</id> 
    <name>Central Repository</name> 
    <url>http://repo.maven.apache.org/maven2</url> 
</repository>
5、maven.org 中央仓库

<repository> 
    <id>central-repos1</id> 
    <name>Central Repository 2</name> 
    <url>http://repo1.maven.org/maven2/</url> 
</repository>
6、alfresco.com 中央仓库（第3推荐使用）

<repository> 
    <id>activiti-repos</id> 
    <name>Activiti Repository</name> 
    <url>https://maven.alfresco.com/nexus/content/groups/public</url> 
</repository> 
7、oschina 中央仓库（需要x墙哟）

<repository> 
    <id>oschina-repos</id> 
    <name>Oschina Releases</name> 
    <url>http://maven.oschina.net/content/groups/public</url> 
</repository> 
8、oschina thinkgem 中央仓库（需要x墙哟）

<repository>  
    <id>thinkgem-repos</id>  
    <name>ThinkGem Repository</name> 
    <url>http://git.oschina.net/thinkgem/repos/raw/master</url> 
</repository>
9、java.net 中央仓库（需要x墙哟）

<repository> 
    <id>java-repos</id> 
    <name>Java Repository</name> 
    <url>http://download.java.net/maven/2/</url> 
</repository>
10、github.com 中央仓库（需要x墙哟）

<repository>  
    <id>thinkgem-repos2</id>  
    <name>ThinkGem Repository 2</name> 
    <url>https://raw.github.com/thinkgem/repository/master</url> 
</repository> 
三、Maven 中央仓库配置示例
这里使用 Dubbo官方的中央仓库为示例，在 settings.xml 的 profiles 节点中添加如下内容：

<profile>
       <id>jdk‐1.8</id>
       <activation>
              <activeByDefault>true</activeByDefault>      
              <jdk>1.8</jdk>
       </activation>
       <properties>
             <maven.compiler.source>1.8</maven.compiler.source>
             <maven.compiler.target>1.8</maven.compiler.target>
       <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
       </properties>
       <!-- dubbo 官方的解决方案 -->
       <repositories>
              <repository>
                     <id>sonatype-nexus-snapshots</id>
              <url>https://oss.sonatype.org/content/repositories/snapshots</url>
                     <releases>
                            <enabled>false</enabled>
                     </releases>
                     <snapshots>
                            <enabled>true</enabled>
                     </snapshots>
              </repository>
       </repositories>
</profile>
四、Maven 阿里云(Aliyun)仓库
Maven 仓库默认在国外， 国内使用难免很慢，我们可以更换为阿里云的仓库。

修改 maven 根目录下的 conf 文件夹中的 settings.xml 文件，在 mirrors 节点上，添加内容如下：

<mirror>

  <id>aliyunmaven</id>

  <mirrorOf>*</mirrorOf>

  <name>阿里云公共仓库</name>

  <url>https://maven.aliyun.com/repository/public</url>

</mirror>
推荐内容





# loveMQY
项目积累

private Poem getPoem () {
        Poem poem = new Poem();
        poem.setAuthor("李白");
        poem.setCategory("唐诗");
        poem.setContent("日照香炉生紫烟遥看瀑布挂前川飞流直下三千尺疑是银河落九天日日");
        poem.setDynasty("唐");
        poem.setHref("");
        poem.setTitle("望庐山瀑布");
        poem.setTranslation("");
        return poem;
    }


private Long id;
    private String dynasty;
    private String category;
    private String title;
    private String author;
    private String content;
    private String href;
    private String translation;

    public class PoemContent {

    private Long id;

    private String sentence;

    private int order;

    private Long poemId;
    }


HanZi
     @TableField("NAME")
    private String name;
    @TableField("EXPLANATORY_NOTE")
    private String explanatoryNote;


    PersonHanZiLink

    @TableField("PERSON_ID")
    private Long personId;
    @TableField("HAN_ZI_ID")
    private Long hanZiId;

public String dealContentHanZiList(String content) {
        Long personId = 100L;
        String errorMsg = StrUtil.EMPTY;
        Map<Long, Integer> contentIdMap = getAllContentIdMap(content);
        List<HanZiVo> deleteList = Lists.newArrayList();
        List<HanZiVo> personHanZiLinkList = getPersonHanZiList(personId);

        //当前字有多个，进行组合
        Map<Long, Integer> personHanZiIdMap = getPersonHanZiIdMap(personHanZiLinkList);

        Map<Long, HanZi> hanZiIdMap = iHanZiService.getHanZiIdMap();
        List<String> canNotSynthesisHanZiList = Lists.newArrayList();
        for (Map.Entry<Long, Integer> entry : contentIdMap.entrySet()) {
            Long key = entry.getKey();
            Integer value = entry.getValue();
            if (personHanZiIdMap.containsKey(key)) {
                if (value > personHanZiIdMap.get(key)) {
                    canNotSynthesisHanZiList.add(hanZiIdMap.get(key).getName());
                }
            } else {
                canNotSynthesisHanZiList.add(hanZiIdMap.get(key).getName());
            }
        }
        if (CollUtil.isNotEmpty(canNotSynthesisHanZiList)) {
            errorMsg = StrUtil.join("", canNotSynthesisHanZiList);
            errorMsg = "这么多字没有:" + errorMsg;
        } else {
            for (Map.Entry<Long, Integer> entry : contentIdMap.entrySet()) {
                Long key = entry.getKey();
                Integer value = entry.getValue();
                for (int i = 0; i < value; i++) {
                    personHanZiLinkList.stream().filter(
                            item -> item.getHanZiId().equals(key)).findFirst().map(vo -> {
                        deleteList.add(vo);
                        personHanZiLinkList.remove(vo);
                        return vo;
                    });
                }
            }
            for (HanZiVo personHanZiLink : deleteList) {
                System.out.println(personHanZiLink.getHanZiId());
                System.out.println(personHanZiLink.getHanZiLinkId());
                System.out.println(personHanZiLink.getName());
                System.out.println("#############");
            }
            List<Long> idsList = deleteList.stream().map(HanZiVo::getHanZiLinkId).collect(Collectors.toList());
            deleteForceByIds(ArrayUtil.toArray(idsList, Long.class));
        }
        return errorMsg;
    }
    
