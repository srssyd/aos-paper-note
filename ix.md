# IX: A Protected Dataplane Operating System for High Throughput and Low Latency �Ķ��ʼ�

Ϊ����ά��OS�ı������Ƶ�ǰ������������ϵͳ��I/O���ܣ�Adam Belay������ʵ����IX��IX��I/O���������linux���˼������ߡ�IX��ӵ�м��ߵ���������ͬʱ��Ҳӵ�кܵ͵���ʱ��

### IX�����

IXΪ���������ܣ��������µķ�����
* ���ں˷�Ϊcontrol plane��data plane��control plane��Ҫ������Դ���ȣ���ص����񣬶�data plane ������������Э��ջ��
* ��ӵ�����Ƶ�ʱ��ʹ��batch
*  ʹ������Э���ܹ���ͷ���е��ף���ʡ��ϵͳ���õĿ�����ͬʱʹ��ϵͳ�ܹ��������cache��
*  ʹ���Լ����е�API��ʵ�������ݵ�zero-copy
*  ����ÿ���߳�ά���Լ��������еĳأ�������ͬ���Ŀ���
*  ���⣬IXʹ����RCU����߶��̷߳���ʱ�Ķ����ܡ�

ͬʱ����IX�У���ΪElastic�̺߳�background�̣߳�elastic�߳��ϵ����в�������non-blocking�ġ�


�ڰ�ȫ�Է��棬IX��application�Ĵ��붼������ring3 �У�data plane����������non root ģʽ��ring 0�С�


### IX������
�������⣬����������㹻���ʱ��IX��������Զʤ��linux��ͬʱҲ����mTCP��user space��network stack��