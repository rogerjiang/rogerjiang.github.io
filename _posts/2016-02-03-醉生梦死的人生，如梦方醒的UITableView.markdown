---
layout: post
title: 醉生梦死的人生，如梦方醒的UITableView
category: iOS
tags: [iOS]
---

## 前言

最近看了一下关于轻量化controller的文章，大致想了一下关于tableView几种写法。

## UITableView的3种写法

### 第一类

是最常见的一种写法，就以当前的controller作为UITableView的delegate。大致写法如下，代理写在controller里面。

<pre>
<code>
@implementation SettingViewController
-(void) loadView
{
    [super loadView];
    UITableView* tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, SCREEN_WIDTH,SCREEN_HEIGHT) style:UITableViewStyleGrouped];
    tableView.delegate = self;
    tableView.dataSource = self;
    self.settingTableView = tableView;
    [self.view addSubview:tableView];
}

#pragma mark-- uitableView Delegate
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;
{
    return self.dataSource.count;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    if(section < 0 || section > self.dataSource.count) {
        return 0;
    }
    NSArray* data = self.dataSource[section];
    return data.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
UITableViewCell* cell = [tableView dequeueReusableCellWithIdentifier:@"recommend"];

    if(cell == nil)
    {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"recommend"];
        cell.contentView.backgroundColor = [UIColor clearColor];
        cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
        SettingCellView* view = [[SettingCellView alloc] init];
        view.tag = 1001;
        [cell.contentView addSubview:view];
    }

    SettingCellView* settingView = (SettingCellView*) [cell.contentView viewWithTag:1001];
    NSInteger row = [indexPath row];
    NSInteger section = [indexPath section];
    SettingCommonModel* model = [self.dataSource[section] objectAtIndex:row];
    [settingView setData:model];

    return cell;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 44;
}
</code>
</pre>

###第二类：

第二种写法，相对第一种少一点。自己定义一个继承UITableView的子类，并且重写UITableView的各种方法。这里还是设置controlller为代理类，具体的实现放在UITableView的子类里面实现，写法有点特点，当然也可以用代理传出来在controller里面实现。
大致写法如下: controller还是不变

<pre>
<code>
@implementation SettingViewController

-(void) loadView
{
    [super loadView];

    SpecialTableView* tableView = [[SpecialTableView alloc] initWithFrame:CGRectMake(0, 0, SCREEN_WIDTH,SCREEN_HEIGHT) style:UITableViewStyleGrouped];
    tableView.delegate = self;
    tableView.dataSource = self;
    self.settingTableView = tableView;

    [self.view addSubview:tableView];
}

</code>
</pre>

TableView的各种方法放在SpecialTableView里面写

<pre>
<code>
@implementation SpecialTableView

#pragma mark-- uitableView Delegate
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;
{
    return self.dataSource.count;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    if(section < 0 || section > self.dataSource.count) {
        return 0;
    }
    NSArray* data = self.dataSource[section];
    return data.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell* cell = [tableView dequeueReusableCellWithIdentifier:@"recommend"];

    if(cell == nil)
    {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"recommend"];
        cell.contentView.backgroundColor = [UIColor clearColor];
        cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
        SettingCellView* view = [[SettingCellView alloc] init];
        view.tag = 1001;
        [cell.contentView addSubview:view];
    }

    SettingCellView* settingView = (SettingCellView*) [cell.contentView viewWithTag:1001];
    NSInteger row = [indexPath row];
    NSInteger section = [indexPath section];
    SettingCommonModel* model = [self.dataSource[section] objectAtIndex:row];
    [settingView setData:model];

    return cell;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 44;
}
</code>
</pre>

第三种写法，也是将UITableView的具体实现放在controller之外，这里不再继承UITableView，但是新写一个专门的类来作为UITalbeView的代理。

代码大致如下：

<pre>
<code>
@implementation SettingViewController

-(void) loadView
{
    [super loadView];

    UITableView* tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, SCREEN_WIDTH,SCREEN_HEIGHT) style:UITableViewStyleGrouped];

    self.tableDelegate = [[TableResultDelegate alloc] init];
    tableView.delegate = self.tableDelegate;
    tableView.dataSource = self.tableDelegate;
    [self.view addSubview:tableView];
}
</code>
</pre>

TableResultDelegate的写法：

<pre>
<code>
@implementation TableResultDelegate

#pragma mark-- uitableView Delegate
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;
{
    return self.dataSource.count;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    if(section < 0 || section > self.dataSource.count) {
        return 0;
    }   
    NSArray* data = self.dataSource[section];
    return data.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell* cell = [tableView dequeueReusableCellWithIdentifier:@"recommend"];

    if(cell == nil)
    {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"recommend"];
        cell.contentView.backgroundColor = [UIColor clearColor];
        cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
        SettingCellView* view = [[SettingCellView alloc] init];
        view.tag = 1001;
        [cell.contentView addSubview:view];
    }

    SettingCellView* settingView = (SettingCellView*) [cell.contentView viewWithTag:1001];

    NSInteger row = [indexPath row];
    NSInteger section = [indexPath section];
    SettingCommonModel* model = [self.dataSource[section] objectAtIndex:row];
    [settingView setData:model];

    return cell;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{   
    return 44;
}
</code>
</pre>

## 关于这3种写法

看了下产品中的代码，这3种都有具体的例子是这么写的，对这几种写法，个人的感觉：

第一种写法是最常见的写法，比较适合一些代码量不大的controller里面的，比如这个页面只有一个UITableView这种场景，这个类的代码结构也是比较清晰的，功能相对比较单一。

如果代码量很大的一个controller，就需要考虑一些轻量化咯。比如一个5，6千行的controller，看的时候会相当的麻烦。所以最好考虑减轻一些controller的负担，就可以把UITableView的这些代理拿出去。

第二种写法相对来说，就是把数据和事件等独立了出来，减轻了controller的负担。可以把UITalbeView作为一个单独的模块。但是觉得继承这个类的话。如果再在controller里面再次写这些代理方法，相对来说会比较混乱(当然也可以不用这些写)。

第三种写法，也是把数据和事件等独立了出来，减轻了controller的负担。对于数据的处理和事件的处理都比较清晰。

第二和第三种写法，还需要考虑一下数据的通信问题，相对来说比较独立一点, 对于比较重的controller，可以考虑这种写法。

