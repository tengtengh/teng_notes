## Ubuntuϵͳ��װ����

�����ص����壨*.ttf��*.TTF�ȣ����������ļ��п�����`sudo cp -rf xxx`����`/usr/share/fonts/`�ļ����С�

ִ����������

```bash
cd /usr/share/fonts/

# ����Ȩ��
sudo chmod -R 755 xxx 

# �����ź������fonts.scale�ļ�������������������ת����
sudo mkfontscale 

# �����ź������fonts.dir�ļ������������������б�����
sudo mkfontdir

# �������建����Ϣ��Ҳ������ϵͳ��ʶ�ź�
sudo fc-cache -fv 
sudo fc-cache 
```
