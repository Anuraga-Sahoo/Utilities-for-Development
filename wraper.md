## Try Catch wraper
make a file named TryCatch.js

```bash
const TryCatch = (handler) => {
  
  return async(req, res, next) => {
    try {
      await handler(req, res, next)
      
    } catch (error) {
      res.status(500).json({
        message: error.message,
        error: error,
        success: false
      })
    }
  }
}

export default TryCatch
```