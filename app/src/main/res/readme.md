HtmlTagHandler
===================

* ����
    * ֧��TextViewĬ��֧�ֵı�ǩ
    * ֧���Զ����ǩ���ӿ�����Html.TagHandler


1 �Զ����ǩ��

```java
public class SpanTagHandler implements HtmlTagHandler.TagHandler {

    private String fontColor = "";

    @Override
    public void handleTag(boolean open, String tag, Editable output, Attributes attrs) {

        if(tag.toLowerCase().equals("span")){
            if(open){
                //����ǩ��output�ǿգ�sax��û��������attrs��ֵ
                for(int i = 0; i < attrs.getLength(); i++){
                    Log.i("22222", "====" + attrs.getLocalName(i) + ": " + attrs.getQName(i) + ": " + attrs.getValue(i));
                    if(attrs.getLocalName(i).equals("style")){
                        String style = attrs.getValue(i); //{color:#e60012}
                        fontColor = style.replace("{", "").replace("}", "").replace("color", "").replace(":", "");
                        Log.i("22222", "fontColor=" + fontColor);
                    }
                }
            }else{
                //�ձ�ǩ��output��ֵ�ˣ�attrsûֵ
                output.setSpan(new ForegroundColorSpan(Color.parseColor(fontColor)), 0, output.length(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
            }
        }//~~over if
    }
}
```

2 htmlת����spanned
```java
String content = "<span style=\"{color:#e60012}\">������</span>";
Spanned s = HtmlTagHandler.fromHtml(content, null, new SpanTagHandler());
tv.setText(s);
```

3 ΪʲôҪ�������

���հ�׿Ĭ���ṩ�ķ�ʽ����TextView��ʾhtml��������
tv_2.setText(Html.fromHtml(content, null, new Html.TagHandler()));

����Html.TagHandler�ṩ�Ľӿ��ǣ�
public void handleTag(boolean opening, String tag, Editable output, XMLReader xmlReader)

span��ǩĬ���ǲ���֧�ֵģ�����Ҫ�Լ�д�Ļ���Ҫ�õ�span��ǩ���ı���style���ԣ���Attributesû��������
�������˸�XmlReader���������XmlReader�����Ҳ�����

��android.text.Html��Դ�룺
handleStartTag(String tag, Attributes attributes)
������ʵ�Ѿ������������ˣ�Ϊ�β��������أ�����

����HtmlTagHandler�͸��������£�һ�ǿ���Դ�룬���Ǹ��Ľӿڣ�ȥ��XmlReader������Attributes