---
title: react-full-page controls customize하기
author:  juyoung
date: 2020-12-07 22:06:00 +0800
categories: [react, portpolio]
tags: [react]
---  
  


#### 1. `<FullPage>` 컴포넌트에 `controls`값으로 CustomControls 컴포넌트를 넣어준다.  

> controls={CustomControls}  
  

#### 2. [react-full-page](https://github.com/zwug/react-full-page#readme)에서 제공하는 Basic controls props인 `getCurrentSlideIndex`, `scrollToSlide`를 인자로 받아온다.

#### 3. 선택된 메뉴의 key인 selectedKeys를 `getCurrentSlideIndex`와 연결해 `<Slide>`가 하나씩 넘어가도록 하고, Menu를 클릭하면 해당 `<Slide>`로 이동한다.  
    
* 아래의 경우 antd에서 제공하는 `Menu.Item`를 클릭하면 onClick으로 클릭한 메뉴의 key를 e.key값으로 받아올 수 있다.    
   

  components > AppLayout.js
```javascript
const CustomControls =({ getCurrentSlideIndex, scrollToSlide }) => {
    const currentSlideIndex = getCurrentSlideIndex();
    const onClickMenu = useCallback((e) => {
        scrollToSlide(e.key - 1);
    }, []);
  
    return (
        <>
            <Menu 
                selectedKeys={[`${currentSlideIndex + 1}`]}
                onClick={onClickMenu}              
            >
                <Menu.Item key="1" icon={
                    <HomeOutlined />}>
                    Home
                </Menu.Item>
                <Menu.Item key="2" icon={
                    <UserOutlined />}>
                    About
                </Menu.Item>
                <Menu.Item key="3" icon={
                    <AppstoreAddOutlined />}>
                    Tech Skills
                </Menu.Item>
                <Menu.Item key="4" icon={
                    <BulbOutlined />
                }>
                    Project
                </Menu.Item>
                <Menu.Item key="5" icon={
                    <FormOutlined />
                }>
                    Experience
                </Menu.Item>
                <Menu.Item key="6" icon={
                    <BankOutlined />}>
                    Education
                </Menu.Item>
                <Menu.Item key="7" icon={
                    <MailOutlined />
                }>
                    Contact
                </Menu.Item>
          </Menu>
        </>
    );
};
const AppLayout = ({ children }) => {
 
    return (
        <>
                   <Layout>
                      <Header></Header>
                        <Content>
                            <div>
                                <FullPage
                                    controls={CustomControls}
                                >
                                    {children}
                                </FullPage>
                            </div>
                        </Content>
                        <Footer></Footer>
                    </Layout>              
        </>
    );
};
AppLayout.propTypes = {
    children: PropTypes.node.isRequired,
};
export default AppLayout;
```

* `<AppLayout>`으로 `<Slide>`를 감싸주고 그 안에 페이지별 컴포넌트를 넣었다.

pages > index.js    

```javascript
 <AppLayout>
            <Slide>
                <div
                    className={isTabletPC ? "basic-layout-background-pc" : "basic-layout-background-mobile"}>
                    <Home />//페이지별 컴포넌트 자리
                </div>
            </Slide>
</AppLayout>
```