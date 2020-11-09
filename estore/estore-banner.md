# EstoreBanner

![EstoreBanner](../images/estore-banner.png)

### Usage
```jsx
import EStoreBanner from '../components/partials/banner/EStoreBanner';

const banner = {
  slides: [
    {
      image: {
        md: '/resources/cbu/estore/estore-banner-1440-x-460@2x.jpg',
        sm: '/resources/cbu/estore/estore-banner-m@2x.jpg',
      },
      tag: '網路限定',
      title: '499吃到飽不限速',
      description: `<h5>iPhone11系列送遠傳friDay90天，再送手機保險3個月</h5>`,
      actions: [{ text: '線上申辦', link: '', btnStyle: 'primary' }],
    },
    {
      image: {
        md: '/resources/cbu/estore/estore-banner-1440-x-460@2x.jpg',
        sm: '/resources/cbu/estore/estore-banner-m@2x.jpg',
      },
      tag: '網路限定',
      title: '499吃到飽不限速',
      description: 'iPhone11系列送遠傳friDay90天，再送手機保險3個月',
      actions: [{ text: '線上申辦', link: '', btnStyle: 'primary' }],
    },
  ],
};

class Index extends Recat.Component {
  render () {
    return (
      <EStoreBanner {...banner} />
    );
  }
}
```

### Sources
```jsx
import React from 'react';
import Slider from 'react-slick';
import Button from '../../Button';

import PropTypes from 'prop-types';

class EStoreBanner extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      isMobile: window.innerWidth <= 768,
    };

    this.slideSetting = {
      dots: true,
      infinite: true,
      arrows: false,
      autoplay: true,
      autoplaySpeed: 5000,
      slidesToShow: 1,
      draggable: true,
      adaptiveHeight: true,
    };
  }

  componentDidMount = () => {
    window.addEventListener('resize', (e) => {
      this.setState({
        isMobile: window.innerWidth <= 768,
      });
    });
  };

  render() {
    return (
      <div className='fui-banner is-estore'>
        <Slider {...this.slideSetting}>
          {this.props.slides.map((slide, i) => (
            <div className='estore-banner-item' key={`estore-banner-${i}`}>
              <div
                className='bg'
                style={{
                  backgroundImage: `url(${this.state.isMobile ? slide.image.sm : slide.image.md})`,
                }}>
                {/* <img src={slide.image.md} alt={slide.title} className='d-none d-sm-block' />
                <img src={slide.image.sm} alt={slide.title} className='d-block d-sm-none' /> */}
              </div>
              <div className='fui-container'>
                <div className='caption'>
                  {slide.tag ? (
                    <div className='tag'>
                      <div className='tag-bg'></div>
                      {slide.tag}
                    </div>
                  ) : null}
                  <h1 dangerouslySetInnerHTML={{ __html: slide.title }}></h1>
                  <div dangerouslySetInnerHTML={{ __html: slide.description }}></div>
                  {/* {Array.isArray(slide.description) ? (
                    <div className='fui-list'>
                      {slide.description.map((desp, i) => (
                        <div className='fui-item' key={`estore-banner-desp-${i}`}>
                          <img
                            src={process.env.PUBLIC_URL + '/resources/cbu/estore-deals/check-2.svg'}
                            width='30'
                            alt='check'
                          />
                          <span className='text' dangerouslySetInnerHTML={{ __html: desp }}></span>
                        </div>
                      ))}
                    </div>
                  ) : (
                    <h5 dangerouslySetInnerHTML={{ __html: slide.description }}></h5>
                  )} */}

                  {slide.actions ? (
                    <div className='action'>
                      {slide.actions.map((act, j) => (
                        <Button
                          link={act.link}
                          target={act.target}
                          btnStyle={act.btnStyle}
                          key={`estore-banner-${i}-btn-${j}`}
                          size='large'>
                          {act.text}
                        </Button>
                      ))}
                    </div>
                  ) : null}
                </div>
              </div>
            </div>
          ))}
        </Slider>
      </div>
    );
  }
}

EStoreBanner.propTypes = {
  slides: PropTypes.arrayOf(
    PropTypes.shape({
      image: PropTypes.shape({
        md: PropTypes.string,
        sm: PropTypes.string,
      }),
      tag: PropTypes.string,
      title: PropTypes.string,
      description: PropTypes.string,
      actions: PropTypes.arrayOf(
        PropTypes.shape({
          text: PropTypes.string,
          link: PropTypes.string,
          target: PropTypes.string,
        })
      ),
    })
  ),
};

export default EStoreBanner;
```

### Properties
<table>
  <thead>
    <tr>
      <th colspan='2'>名稱</th>
      <th>屬性</th>
      <th>必填</th>
      <th>選項</th>
      <th>說明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan='2'>
        slides
      </td>
      <td>Array</td>
      <td>true</td>
      <td></td>
      <td>可輪播的 Banner 內容</td>
    </tr>
    <tr>
      <td>
      <td>
        tag 
      </td>
      <td>String</td>
      <td></td>
      <td></td>
      <td>Banner 上方標籤文字</td>
    </tr>
    <tr>
      <td>
      <td>
        title 
      </td>
      <td>String</td>
      <td>true</td>
      <td></td>
      <td>標題</td>
    </tr>
    <tr>
      <td>
      <td>
        description 
      </td>
      <td>String</td>
      <td>true</td>
      <td></td>
      <td>描述</td>
    </tr>
    <tr>
      <td>
      <td>
        actions 
      </td>
      <td>Object</td>
      <td></td>
      <td></td>
      <td>
{<br/>
  text: 按鈕文字<br/>
  link: 按鈕連結<br/>
  target: _self | _blank // 直接跳頁或另開視窗，預設為 _self<br/>
}
      </td>
    </tr>
  </tbody>
</table>

