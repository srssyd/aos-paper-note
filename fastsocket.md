# Scalable Kernel TCP Design and Implementation for Short-Lived Connections �Ķ��ʼ�

��ƪ���������Fastsocket��һ�������ں�̬����Socket�ӿڼ��ݵ�TCPЭ��ջ������24��CPU���ܹ�ʵ��20.4���ļ��ٱȣ�����ҪԶ����linux��ʵ�֡�

### ���е�TCP��ʵ�ֵ�ƿ��

ĿǰTCP��ʵ����������ƿ����
* TCB�Ĺ�����������Global Listen Table��Global Established Table�Ĵ��ڣ��д��������Ŀ�����
* ȱ��connection locality������һ��connection�������İ��п����ڲ�ͬ��CPU�ϱ�����,�ᵼ�������½���
* VFS�ļ�ϵͳ���������Ŀ������ر��Ƕ�inodes��dentries�Ĺ��������������ͬ��������

### FastSocket�����

FastSocket����ƴ�����£�
1. ��global��TCB�ı����CPU��ȫ���ֿ�����
2. ʹ��Receive Flow Deliver��������������Ӧ��CPU��
3. ��VFS��ʶ��Fastsocket�Ĵ��ڣ�����˼�������Ҫ�Ŀ�����

### FastSocket��ʵ��ϸ��

ÿ��CPU���Լ���Ӧ��Listen Table,��ȻҲ��global��table����������£�socket��ֱ��ͨ���Լ���������Ӧ��Listen Table����Ӧ�����ǣ���һ��Process crash�˻����������ʱ���ͻ����global��table�������Ӧ���߼���

����RFD,�ڸ��ݰ��Ķ˿ڽ��з���֮��ͨ��������ATR���԰����й��˺ͷַ���

����VFS,���ǿ���֪��dentry��inode���ݽṹ����socket��û�кܶ����õģ�����Fastsocket�����˺ܶ�denty��inode�ĳ�ʼ�����̡���Ȼ��Ϊ�˱��ֶ�proc�ļ�ϵͳ�ļ��ݣ�һ���ֵĹ��ܻ�������������


