---
title: LintCode-中位数
date: 2015-10-04 21:01:53
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/48899829   
  中位数

 给定一个未排序的整数数组，找到其中位数。

 中位数是排序后数组的中间值，如果数组的个数是偶数个，则返回排序后数组的第N/2个数。

 给出数组[4, 5, 1, 2, 3]， 返回 3

 给出数组[7, 9, 4, 5]，返回 5

 挑战   
 时间复杂度为O(n)

 
```
public class Solution {
    /**
     * @param nums: A list of integers.
     * @return: An integer denotes the middle number of the array.
     */
     public static int median(int[] nums) {
        int left=0,right=nums.length-1,key=right/2,k=0;
        while(key!=k){
            k=sort(nums,left,right);
            if(k>key){
                right=k-1;
            }else{
                left=k+1;
            }
        }
        return nums[key]; 

    }

    public static int sort(int arr[],int low,int high){
        int l=low;
        int h=high;
        int povit=arr[low];
        while(l<h){
            while(l<h&&arr[h]>=povit)
                h--;
            if(l<h){
                swap(arr,l,h);
                l++;
            }

            while(l<h&&arr[l]<=povit)
                l++;
            if(l<h){
                swap(arr,l,h);
                h--;
            }
        }
        return h;

    }

    public static void swap(int [] nums ,int a,int b){
        int k=nums[a];
        nums[a]=nums[b];
        nums[b]=k;
    }
}


```
 主要利用快排递归划分的思想，可以在期望复杂度为O(n)的条件下求第k大数。快排的期望复杂度为O(nlogn)，因为快排会递归处理划分的两边，而求第k大数则只需要处理划分的一边，其期望复杂度将是O(n)。

 
```
快排
```
 设要排序的数组是A[0]……A[N-1]，首先任意选取一个数据（通常选用数组的第一个数）作为关键数据，然后将所有比它小的数都放到它前面，所有比它大的数都放到它后面，这个过程称为一趟快速排序。值得注意的是，快速排序不是一种稳定的排序算法，也就是说，多个相同的值的相对位置也许会在算法结束时产生变动。   
 一趟快速排序的算法是：   
 1）设置两个变量i、j，排序开始的时候：i=0，j=N-1；   
 2）以第一个数组元素作为关键数据，赋值给key，即key=A[0]；   
 3）从j开始向前搜索，即由后开始向前搜索(j–)，找到第一个小于key的值A[j]，将A[j]和A[i]互换；   
 4）从i开始向后搜索，即由前开始向后搜索(i++)，找到第一个大于key的A[i]，将A[i]和A[j]互换；   
 5）重复第3、4步，直到i=j； (3,4步中，没找到符合条件的值，即3中A[j]不小于key,4中A[i]不大于key的时候改变j、i的值，使得j=j-1，i=i+1，直至找到为止。找到符合条件的值，进行交换的时候i， j指针位置不变。另外，i==j这一过程一定正好是i+或j-完成的时候，此时令循环结束）。

 
```
class　Quick
{
　public　void　sort(int　arr[],int　low,int　high)
　{
　int　l=low;
　int　h=high;
　int　povit=arr[low];

　while(l<h)
　{
　while(l<h&&arr[h]>=povit)
　h--;
　if(l<h){
　int　temp=arr[h];
　arr[h]=arr[l];
　arr[l]=temp;
　l++;
　}

　while(l<h&&arr[l]<=povit)
　l++;

　if(l<h){
　int　temp=arr[h];
　arr[h]=arr[l];
　arr[l]=temp;
　h--;
　}
　}
　print(arr);
　System.out.print("l="+(l+1)+"h="+(h+1)+"povit="+povit+"\n");
　if(l>low)sort(arr,low,l-1);
　if(h<high)sort(arr,l+1,high);
　}
}


/*//////////////////////////方式二////////////////////////////////*/
更高效点的代码：
public<TextendsComparable<?superT>>
T[]quickSort(T[]targetArr,intstart,intend)
{
inti=start+1,j=end;
Tkey=targetArr[start];
SortUtil<T>sUtil=newSortUtil<T>();

if(start>=end)return(targetArr);


/*从i++和j--两个方向搜索不满足条件的值并交换
*
*条件为：i++方向小于key，j--方向大于key
*/
while(true)
{
while(targetArr[j].compareTo(key)>0)j--;
while(targetArr[i].compareTo(key)<0&&i<j)i++;
if(i>=j)break;
sUtil.swap(targetArr,i,j);
if(targetArr[i]==key)
{
j--;
}else{
i++;
}
}

/*关键数据放到‘中间’*/
sUtil.swap(targetArr,start,j);

if(start<i-1)
{
this.quickSort(targetArr,start,i-1);
}
if(j+1<end)
{
this.quickSort(targetArr,j+1,end);
}

returntargetArr;
}


/*//////////////方式三：减少交换次数，提高效率/////////////////////*/
private<TextendsComparable<?superT>>
voidquickSort(T[]targetArr,intstart,intend)
{
inti=start,j=end;
Tkey=targetArr[start];

while(i<j)
{
/*按j--方向遍历目标数组，直到比key小的值为止*/
while(j>i&&targetArr[j].compareTo(key)>=0)
{
j--;
}
if(i<j)
{
/*targetArr[i]已经保存在key中，可将后面的数填入*/
targetArr[i]=targetArr[j];
i++;
}
/*按i++方向遍历目标数组，直到比key大的值为止*/
while(i<j&&targetArr[i].compareTo(key)<=0)
/*此处一定要小于等于零，假设数组之内有一亿个1，0交替出现的话，而key的值又恰巧是1的话，那么这个小于等于的作用就会使下面的if语句少执行一亿次。*/
{
i++;
}
if(i<j)
{
/*targetArr[j]已保存在targetArr[i]中，可将前面的值填入*/
targetArr[j]=targetArr[i];
j--;
}
}
/*此时i==j*/
targetArr[i]=key;

/*递归调用，把key前面的完成排序*/
this.quickSort(targetArr,start,i-1);


/*递归调用，把key后面的完成排序*/
this.quickSort(targetArr,j+1,end);

}
```
   
  