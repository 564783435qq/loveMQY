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
    
