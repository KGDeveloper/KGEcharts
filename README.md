# KGEcharts
一个扇形统计图以及折线统计，后续会不断更新其他统计图，并且完善现有功能以及新增拓展性

<Img src="http://chuantu.xyz/t6/702/1566377491x1033347913.png"/> <Img src="http://chuantu.xyz/t6/702/1566377985x1033347913.png"/>

1.扇形统计图KGPieView

2.折线统计图KGLineView

3.使用示例：
```

#import "KGChartViewController.h"
#import <KGEcharts/KGEcharts.h>

//在此遵守数据设置代理
@interface KGChartViewController ()<KGPieViewDelegate,KGPieViewDataSource,KGLineViewDataSource,KGLineViewDelegate>

//扇形统计图
@property (nonatomic,strong) KGPieView *pieView;
//折线统计图
@property (nonatomic,strong) KGLineView *lineView;

@end

@implementation KGChartViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self.pieView reloadData];
    [self.view bringSubviewToFront:self.lineView];
    
}

- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    [self.pieView reloadData];
}

//懒加载初始化扇形统计图
- (KGPieView *)pieView{
    if (!_pieView) {
        _pieView = [[KGPieView alloc] initWithFrame:CGRectMake(0, KGNavHeight + 15, KGScreen_Width, 150)];
        _pieView.delegate = self;
        _pieView.dataSource = self;
        [self.view addSubview:_pieView];
    }
    return _pieView;
}

//设置扇形统计图区域颜色
- (UIColor *)pieChartView:(KGPieView *)pieChartView colorForSliceAtIndex:(NSInteger)index{
    return [self pieChartViewSlieViewColor][index];
}

//设置扇形统计图每个区域大小
- (CGFloat)pieChartView:(KGPieView *)pieChartView valueForSliceAtIndex:(NSInteger)index{
    return 60.0f;
}
//设置扇形统计图分区数
- (NSInteger)numberOfSlicesInPieChartView:(KGPieView *)pieChartView{
    return 5;
}
//设置扇形统计图图标
- (NSString *)pieChartView:(KGPieView *)pieChartView titleForSliceAtIndex:(NSInteger)index{
    return [self pieChartsViewSlieViewTitle][index];
}
//扇形统计图中心圆半径
-(CGFloat)centerCircleRadius{
    return 0;
}

- (NSArray *)pieChartViewSlieViewColor{
    return @[[KGToolManager colorWithHexString:@"#00FF00"],[KGToolManager colorWithHexString:@"#FFFF00"],[KGToolManager colorWithHexString:@"#FF8C00"],[KGToolManager colorWithHexString:@"#FF0000"],[KGToolManager colorWithHexString:@"#00BFFF"]];
}

- (NSArray *)pieChartsViewSlieViewTitle{
    return @[@"北京",@"上海",@"广州",@"深圳",@"天津"];
}

//懒加载初始化折线统计图
- (KGLineView *)lineView{
    if (!_lineView) {
        _lineView = [[KGLineView alloc] initWithFrame:CGRectMake(0, KGNavHeight + 180, KGScreen_Width, 300)];
        _lineView.backgroundColor = [UIColor whiteColor];
        _lineView.delegate = self;
        _lineView.dataSource = self;
        [self.view addSubview:_lineView];
    }
    return _lineView;
}
//设置Y坐标分区个数
- (NSInteger)numberOfSliceInYAxis{
    return 5;
}
//设置X坐标分区个数
- (NSInteger)numberOfSliceInXAxis{
    return 5;
}
//设置Y坐标轴分区标题
- (NSString *)lineView:(KGLineView *)lineView titleForYAxisAtIndex:(NSInteger)index{
    return [self yAxisTitle][index];
}
//设置X坐标轴分区标题
- (NSString *)lineView:(KGLineView *)lineView titleForXAxisAtIndex:(NSInteger)index{
    return [self xAxisTitle][index];
}
//设置折线统计图每个分区峰值
- (float)lineView:(KGLineView *)lineView valueForXAxisAtIndex:(NSInteger)index{
    NSDictionary *dic = [self lineValue][index];
    return [dic[@"value"] floatValue];
}
//设置是否在每个分区位置显示峰值
- (BOOL)lineView:(KGLineView *)lineView isShowTitleForLineAtIndex:(NSInteger)index{
    return index==3?NO:YES;
}
//设置每个分区位置峰值显示内容
- (NSString *)lineView:(KGLineView *)lineView showTitleForLineAtIndex:(NSInteger)index{
    NSDictionary *dic = [self lineValue][index];
    return [NSString stringWithFormat:@"%@",dic[@"value"]];
}
//Y坐标轴头
- (NSString *)theYAxisTitle{
    return @"百分比(%)";
}
//X坐标轴头
- (NSString *)theXAxisTitle{
    return @"分类";
}

- (NSArray *)yAxisTitle{
    return @[@"20",@"40",@"60",@"80",@"100"];
}

- (NSArray *)xAxisTitle{
    return @[@"吃",@"穿",@"住",@"行",@"娱乐"];
}

- (NSArray*)lineValue{
    return @[@{@"value":@20.0f},@{@"value":@60.0f},@{@"value":@55.0f},@{@"value":@89.0f},@{@"value":@12.0f}];
}

@end

```
